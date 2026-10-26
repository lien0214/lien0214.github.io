---
title: "DRPS: Per-Agent Rotation in Cooperative MARL"
description: "Parameter sharing in multi-agent RL lets agents differ only in how they scale shared features. DRPS also lets each agent rotate them, and shows that rotation is a controllable axis of specialization."
date: 2026-07-01
period: "Jul 2026 – Oct 2026"
team: "IIS, Academia Sinica"
category: "Research"
status: "AAMAS 2027 under review"
stack: ["Multi-Agent RL", "Parameter Sharing", "QMIX", "SMAC", "PyTorch"]
---

## The Problem

Cooperative multi-agent reinforcement learning usually shares one policy network across all agents. That is sample-efficient, but shared weights push agents toward the same behavior even when the task needs them to take on different roles.

Recent methods (HPS, Prism) keep the network shared and give each agent its own *diagonal* coefficients in a shared feature frame. Agents can then differ in how much they scale each feature, but not in how they orient the features.

## What We Did

- **One coordinate system for existing methods.** Decomposing a per-agent linear modulation into rotation, diagonal and coupling parts shows that both HPS and Prism lack a rotating factor.
- **DRPS (Diagonal-Rotation Parameter Sharing)** adds a learnable, per-agent block-diagonal rotation on top of the diagonal gate. The Cayley map turns unconstrained parameters into exact rotations, and the block size controls how much rotation is allowed. With block size 1 it reduces exactly to HPS, so "rotation" and "no rotation" are two points on one curve.
- At the default block size the rotation adds only **6.7% more parameters** than HPS.

## Results

- On **eight SMAC maps**, rotation makes the win rate rise earlier on nearly every map, and final performance is at or above HPS on seven of them. Blocks that are too large lose ground late on two maps.
- **Controls** separate the effect from parameter count: frozen rotations with matched parameter counts fall *below* the baseline, so the gains are not just extra parameters.
- **Honest limit:** on composition-randomized SMACv2 scenarios, DRPS finishes below HPS. The per-agent part of the rotation seems to need a stable mapping from agent to unit.

The paper is under double-blind review at AAMAS 2027; code will be released on publication.
