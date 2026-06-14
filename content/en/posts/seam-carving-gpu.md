---
title: "We Tried to Make Seam Carving Fast on a GPU. Then We Proved Why It Can't Be."
date: 2026-06-14
description: "A profiling-driven study of exact seam carving on an NVIDIA V100 — optimizing Naive → Fused → Prefetch → BytePtr → Tiled, reaching 5.73 ms/seam at 8K, and then proving the ceiling is structural."
tags:
  - cuda
  - parallel-programming
  - research
  - gpu
  - systems
categories:
  - Engineering
showToc: true
cover:
  image: /images/posts/seam-carving.png
  alt: "Original image, seam paths highlighted in red, and the content-aware carved result"
  relative: false
  hidden: false
  hiddenInSingle: false
---

> Full paper: [seam_carving.pdf](https://github.com/YuXiangLo/NTUPDP2026/blob/main/paper/seam_carving.pdf) · Code: [YuXiangLo/NTUPDP2026](https://github.com/YuXiangLo/NTUPDP2026)  
> Joint work with Ya-Chen Wu and Yu-Hsiang Lo, NTU Parallel Programming, Spring 2026.

## The Starting Point

Seam carving is a content-aware image resizing algorithm. Instead of cropping or scaling, it removes the *least important* pixel paths — seams — from the image, leaving the interesting parts intact. The results look surprisingly good.

The algorithm has two heavy stages. First, compute an energy map (gradient magnitude, one pass over the image). Then, run a dynamic program (DP) to find the lowest-energy connected path from top to bottom. Remove the seam. Repeat.

On a CPU, this is slow for large images. A single 8K frame (7680×4320, ~33 million pixels) can take hundreds of milliseconds per seam on one core. GPUs exist. We have a V100. The energy stage is embarrassingly parallel. The question seemed obvious: how much faster can we make this?

We expected to find a novel optimization and land a big number. We ended up proving why a big number is structurally impossible — and that turned out to be the more interesting result.

## The Problem With the DP

Profiling the pipeline on a 1428×968 image with Nsight Compute reveals the problem immediately: the DP kernel consumes ~93% of per-seam wall time. Everything else is noise.

The DP is:

```
M[i][j] = e[i][j] + min(M[i-1][j-1], M[i-1][j], M[i-1][j+1])
```

Each row depends on the row above it. All H rows are serial. Within each row, the W columns are parallel — but you cannot start row `i` until row `i-1` is complete, and no reordering eliminates this. This is the wall we kept running into.

## The Optimization Path

We built five progressively better kernels, guided by Nsight at each step.

**Naive** launches one CUDA kernel per row. That's H−1 kernel launches per seam, with a full global-memory round-trip each time. At 4K (H=2160), that's 2159 kernel launches for one seam. Overhead dominates on small images; global memory traffic dominates on large ones.

**Fused** collapses all H rows into a single kernel using shared-memory ping-pong buffers. One `__syncthreads()` between rows. One kernel launch replaces 2159. The cost array never hits DRAM between rows — it lives in the 96 KB shared memory budget. This is the right structure; the question is how fast we can make it.

**Prefetch** came from reading Nsight output, not intuition. The dominant stall on Fused was `long_scoreboard` — every warp was waiting ~200 cycles for the global load of the current energy row `e[i][·]`. The fix is software pipelining: while computing row `i` (which only reads shared-memory cost data), each thread issues the global load for `e[i+1][·]` into a register. By the time `__syncthreads()` completes, the load has resolved. Arithmetic and memory transfer overlap. This gave the **largest single jump: 1.67× over Fused** at the kernel level.

**BytePtr** noticed that back-pointers — one of `{-1, 0, +1}` — were stored as 4-byte `int`. Switching to `signed char` cuts back-store traffic 4× at no cost in expressiveness. At 4K (3840×2160), the back-pointer array shrinks from 32 MB to 8 MB per seam pass.

**Tiled** is the piece that finally activated the other 79 SMs we'd been leaving idle. The idea: divide the W columns into K=60 strips (one per SM). Each strip gets T halo columns on each side. Within a tile of T rows, one block works entirely in shared memory, only writing its center strip to global memory at the tile boundary.

The correctness argument is a seam-drift bound: within T rows, a seam shifts at most T columns. Any error stays inside the halo. The center is bit-exact.

The result: kernel launches drop from H to ⌈H/T⌉ (68 at 8K with T=64), and all 60 SMs are active simultaneously. Output is bit-identical to the single-block designs.

## The Numbers

At 8K (7680×4320), Tiled reaches **5.73 ms/seam** — the best exact result across all our designs.

| Method | ctrl | 1080p | 2K | 4K | 6K | 8K |
|--------|------|-------|----|----|----|-----|
| CPU 1 core | 5.00 | 21.6 | 37.5 | 83.6 | 212 | 325 |
| CPU best (32-thread) | 1.14 | 3.59 | 5.63 | 9.88 | 25.6 | 35.4 |
| Naive | 1.37 | 3.10 | 3.77 | 5.92 | 10.3 | 13.9 |
| Tiled (ours) | **0.314** | **0.917** | **1.25** | **2.16** | **4.16** | **5.73** |
| vs CPU best | 3.6× | 3.9× | 4.5× | 4.6× | 6.2× | **6.2×** |

57× over a single CPU core. 6.2× over a tuned 32-thread NUMA-aware OpenMP baseline. That last number is the ceiling — and the reason it's a ceiling is the interesting part.

## Three Reasons the Ceiling Is Structural

At this point, a reasonable question is: are we just not trying hard enough? We spent a fair amount of time asking the same question. The answer is no, and we can show it three ways.

**1. The DP critical path is irreducible.**  
The dependency graph has a critical path of length H — one node per row. Any schedule needs at least H sequential min-steps. A GPU can parallelize within each row (the W columns), but it cannot reduce the H serial steps. The best possible per-row parallelism is already what Tiled achieves.

**2. A transpose cannot help.**  
A natural thought: rotate the image, run the DP in the other direction. But a transpose is a bijection on storage addresses — it relabels storage without changing the dependency graph. The critical path stays H. If you transpose and then carve, you're computing horizontal seams (depth W instead of H), which are just the orthogonal task — not a faster version of the original one. We confirm this empirically: the transposed direction is *slower*, because W > H for wide images.

**3. Removing the DP entirely gives no speedup.**  
This was the most surprising result. We implemented a greedy non-cumulative selection — no cost matrix, no serial dependency, fully parallel. It matches Tiled within 3% at every resolution. Why? Because the per-seam pipeline, not the DP, sets the end-to-end latency. At 8K, 5.73 ms splits into ~4.3 ms DP and ~1.4 ms preprocessing. The preprocessing floor (energy compute + seam removal) requires reading every pixel's energy and writing the output — that's ~398 MB at 8K, and against the V100's ~900 GB/s bandwidth, it costs ~1.4 ms regardless of what selection method you use. Greedy has no DP, but it still pays the same bandwidth floor. It can't win.

## The One Lever That Works

There is one escape: change the problem.

Approximate batch removal amortizes one DP pass over K=60 seams. One forward pass produces 60 strip-local seam candidates; a single kernel removes all 60 in one image-wide read/write pass. This changes the per-seam cost from Θ(HW) per seam to Θ(HW/K).

The result: **0.103 ms/seam at 8K — 56× over the exact single-seam pipeline.** The trade-off is strip-local, not global, optimality: the seams found are each the best within their strip, not provably the global minimum.

This is the batch mode that production pipelines actually use. The exact per-seam formulation isn't the important case; we study it because it's the case where the structural limits are cleanest to analyze.

## What We Actually Learned

We went in expecting to beat prior GPU work with a novel technique. We did build the strongest exact pipeline we could find — Tiled is faster than Kim et al.'s exact single-block DP at every resolution, and Nsight-guided latency hiding made an approach they abandoned competitive again. But 6.2× over a tuned CPU is not the 76× headline their approximate method achieves.

The more valuable output is understanding *why*. The DP's critical path is irreducible. A transpose doesn't help. Removing the DP doesn't help. The bandwidth floor exists regardless of algorithm. These aren't failures of the implementation — they're properties of the problem.

Sometimes the most useful result is a tight characterization of the achievable frontier, with impossibility arguments that explain why no further engineering will move it. We spent more time proving that the ceiling is real than we did hitting it, and I think that's the right way to read the paper.

The one large lever is amortization. If you're willing to trade global optimality for strip-local optimality, batch mode gives you 56×. If you need exact results, 6.2× over a strong multicore CPU is close to what's achievable on a V100 — and the gap between that and the bandwidth floor (~1.3×) tells you there isn't much left.
