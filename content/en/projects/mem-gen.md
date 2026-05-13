---
title: "ICC Mem/Gen"
description: "When a model fine-tunes to believe X but its pretraining says Y — which wins, and can you steer that without retraining?"
date: 2026-03-01
period: "Mar 2026 – Jun 2026"
team: "MSLab · 4-person team"
category: "Research"
status: "NeurIPS 2026 under review"
github: "https://github.com/ntumslab/mem_gen_following"
stack: ["LLM", "LoRA", "PEFT", "Activation Steering", "Python"]
---

## The Problem

When a language model has been fine-tuned to believe X, but its pretraining says Y — which wins? And more importantly, can you steer that behavior without retraining?

## What We Built

**Cross-format data augmentation pipeline** for ConflictQA and ConflictBank. Generates semantically equivalent question variants to stress-test knowledge conflict handling across paraphrase and format shifts — essential for building benchmarks that don't overfit to surface form.

**Activation steering experiments**: extracted fine-tuned behavioral vectors from LoRA-trained models and applied them to frozen base models. The behavioral direction is surprisingly transferable — a steering vector from a LoRA-trained model can push a base model's completions in the expected direction with zero weight updates.

## Key Finding

Fine-tuning injects **directional bias into the residual stream** in a way that's partially separable from task knowledge. If you can steer behavior without weights, you can also potentially *undo* fine-tuning signals. This has direct implications for alignment work.

## Status

Paper under review — NeurIPS 2026.
