---
title: "Seam Carving on a V100"
description: "Profiling-driven CUDA optimization of exact seam carving: five kernels from Naive to Tiled, 5.73 ms/seam at 8K, and proofs that the remaining ceiling is structural. Top 10 of 40 teams."
date: 2026-06-14
period: "Jun 2026"
team: "3-person team · Main Contributor"
category: "Project"
status: "Top 10 / 40 teams · NTU Parallel Programming"
github: "https://github.com/YuXiangLo/NTUPDP2026"
stack: ["CUDA", "C++", "OpenMP", "Nsight", "GPU", "Parallel Programming"]
cover:
  image: /images/posts/seam-carving.png
  alt: "Original image, seam paths highlighted in red, and the content-aware carved result"
---

![Seam carving: original, seams, carved result](/images/posts/seam-carving.png)

## What It Is

Seam carving resizes an image by repeatedly removing the lowest-energy connected path of pixels. It has two heavy stages per seam: an embarrassingly parallel energy map, and a dynamic program whose rows depend on each other. On an NVIDIA V100, we asked how fast the exact algorithm can go.

## What We Built

Five progressively faster kernels, each driven by what Nsight showed was the bottleneck:

- **Naive**: one kernel launch per row, global-memory round trip every row
- **Fused**: all rows in one kernel with shared-memory ping-pong buffers
- **Prefetch**: software-pipelined load of the next energy row to hide `long_scoreboard` stalls (the biggest single jump, 1.67×)
- **BytePtr**: back-pointers stored as `signed char` instead of `int`, cutting that traffic 4×
- **Tiled**: column strips with halos so all SMs work at once, still bit-exact

## Results

- **5.73 ms/seam at 8K**: 57× over a single CPU core, 6.2× over a tuned 32-thread NUMA-aware OpenMP baseline
- Showed the ceiling is structural: the DP critical path is irreducible, a transpose cannot help, and removing the DP entirely still hits the memory-bandwidth floor
- An approximate batch mode amortizes one DP pass over 60 seams and reaches **0.103 ms/seam** (56× faster)

Awarded **Top 10 out of 40 teams** in the NTU Parallel and Distributed Programming final project contest.

[Read the full write-up →](/en/posts/seam-carving-gpu/) · [Paper (PDF)](https://github.com/YuXiangLo/NTUPDP2026/blob/main/paper/seam_carving.pdf)
