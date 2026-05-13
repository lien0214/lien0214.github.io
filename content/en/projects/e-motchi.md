---
title: "E-MoTchi — Emoji ↔ Mandarin NLP"
description: "Fine-tuned multilingual LLMs for bidirectional translation between emoji sequences and natural Traditional Mandarin, with a from-scratch dataset built around Taiwan internet culture."
date: 2024-11-01
period: "Nov 2024 – Dec 2024"
team: "5-person team"
category: "Research"
github: "https://github.com/lien0214/E-MoTChi"
stack: ["Taiwan-LLaMA", "mT5", "LoRA", "RLHF", "NLP", "Python"]
---

## The Task

Bidirectional translation between emoji sequences and Traditional Mandarin Chinese — not dictionary lookup, but contextual understanding of how emojis carry meaning in Taiwan internet culture.

## Dataset

Built from scratch by crowdsourcing emoji-to-Mandarin mappings and applying rule-based augmentation. Key challenge: emoji meaning is deeply contextual and varies by platform, age group, and Taiwan-specific internet slang. Three dataset splits with different algorithmic/GPT-generated data ratios (20/80, 50/50, 80/20) to evaluate data composition effects.

## Models and Results

Fine-tuned **Taiwan-LLaMA** and **Google mT5** using LoRA with RLHF-style preference optimization.

**mT5 outperformed LLaMA** on this task due to its explicit multilingual pretraining on Traditional Chinese text.

**Most interesting finding**: models that scored well on BLEU performed noticeably worse on human evaluation for "feels natural." A clean example of reward hacking — the metric diverges from the goal. We added METEOR evaluation and human annotation rounds to catch this gap.
