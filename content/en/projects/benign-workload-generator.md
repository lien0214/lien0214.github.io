---
title: "Benign Workload Generator"
description: "A toolkit for generating realistic benign Linux server workloads — built to produce high-quality training data for GAN-based intrusion detection models at Academia Sinica."
date: 2023-09-01
period: "Sep 2023 – Jun 2024"
team: "Academia Sinica · Citi Lab"
category: "Research"
github: "https://github.com/lien0214/Benign-Workload-Generator"
stack: ["Python", "Shell", "Cybersecurity", "GAN", "Synthetic Data", "Linux"]
---

## Context

Part of the larger intrusion detection research at Academia Sinica's Citi Lab. The core goal was improving IDS models by augmenting training data with realistic synthetic Linux audit logs — but to generate useful synthetic *attacks*, you first need realistic synthetic *benign* behavior to train against.

## What It Does

**Server workload scripts**:
- `pure.sh` — generates repetitive HTTP requests (GET, POST, PUT) with configurable parameters
- `scenario_combine.sh` — dynamic workloads with randomized request types and concurrent operations, closer to real traffic patterns

**System workload scripts**:
- `main-pure-task.sh` — orchestrates hard disk I/O, virtual memory pressure, matrix computations, and process creation stress tests (~2 min each)
- Individual scripts targeting async I/O, memory allocation, and timer operations

## Why It Matters

A GAN that generates "realistic" logs is only useful if the synthetic attacks fool a detector trained on real data. The quality of the benign baseline directly determines whether the synthetic attack data is worth anything. Getting the workload distribution right — not just the syntax of log entries — was the core challenge.

First real exposure to the gap between academic benchmarks and operational reality in security ML.
