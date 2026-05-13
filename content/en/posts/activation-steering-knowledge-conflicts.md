---
title: "Activation Steering Across Fine-Tuned Models: What We Found in ICC Mem/Gen"
date: 2026-05-10
description: "Can you extract a behavioral direction from a LoRA-finetuned model and apply it to the frozen base? Yes — and the transferability is surprisingly robust. Notes from our ICC Mem/Gen research sprint."
tags:
  - research
  - LLM
  - activation-steering
  - LoRA
  - knowledge-conflict
  - MSLab
categories:
  - Research Notes
showToc: true
---

## Background

The ICC Mem/Gen project in MSLab started with a deceptively simple question: **what does a language model "believe" when its fine-tuning and its pretraining disagree?**

The immediate practical motivation was ConflictQA and ConflictBank — benchmarks specifically designed to surface cases where a model trained on one set of facts encounters questions that probe conflicting facts. Standard fine-tuning on these benchmarks gives you a model that handles the training distribution well but behaves inconsistently on paraphrases or format-shifted variants.

Our angle was different. Instead of just measuring conflict, we wanted to *steer* behavior — and specifically, to understand whether the directional signal introduced by fine-tuning is separable from the task knowledge it's paired with.

---

## The Core Idea: Behavioral Vectors as Transferable Directions

The conceptual framework comes from *Representation Engineering* (Zou et al., 2023). The key claim: certain high-level behaviors (honesty, toxicity, sycophancy) correspond to directions in activation space that are relatively stable across layers and contexts. You can find these directions by contrasting activations on stimuli that elicit the behavior vs. stimuli that don't, and then use those directions as steering vectors.

We adapted this for the fine-tuning case:

1. Take a base model $M_{\text{base}}$ and a LoRA-fine-tuned variant $M_{\text{ft}}$ trained on one of the ConflictQA splits.
2. For a set of conflict-inducing prompts, collect residual stream activations at layer $L$ from both models.
3. Compute the mean difference: $\vec{v} = \mathbb{E}[h_L^{\text{ft}}(x)] - \mathbb{E}[h_L^{\text{base}}(x)]$
4. Apply $\vec{v}$ to the base model's forward pass at inference time: $h_L' = h_L + \alpha \cdot \hat{v}$

The question is whether this makes the base model behave more like the fine-tuned model on conflict questions — **without any weight updates**.

---

## What We Found

Short answer: yes, it works, and it works better than we expected.

**Transfer is layer-dependent.** The steering vector is most effective when extracted and applied at middle-to-late layers (roughly 40–70% of depth for the models we tested). Early layers seem to carry more syntactic/positional information; late layers are too close to the vocabulary projection to generalize well. Middle layers are where the behavioral direction lives.

**The direction is robust to paraphrase.** We computed steering vectors on one paraphrase set and tested on held-out paraphrases of the same conflicts. The transfer held up well. This is actually the non-obvious part — if the vector were picking up on surface features of the specific prompts, you'd expect degradation on paraphrases. The robustness suggests the direction is encoding something more semantic.

**Scale of $\alpha$ matters more than we expected.** With $\alpha$ too small, you see no effect. With $\alpha$ too large, the model produces incoherent outputs (the activation space is being pushed into unexplored territory). There's a roughly 3× range of $\alpha$ where the effect is clean. Calibrating this without a held-out set is annoying.

**Not all conflict types respond equally.** The transfer worked best on factual conflicts (conflicting named entity associations). It worked less well on reasoning conflicts — cases where the fine-tuned model had learned a different inference pattern, not just different facts. This is suggestive: factual knowledge might be more localized in activation space than reasoning strategies.

---

## The Data Augmentation Side

The steering experiments needed a controlled dataset to work on. We built a cross-format augmentation pipeline on top of ConflictQA and ConflictBank:

- **Paraphrase expansion**: for each QA pair, generate 4–6 semantically equivalent paraphrases using a separate LLM (we used Claude Sonnet as the paraphraser). Filter by BLEURT score to ensure semantic equivalence without surface-level overlap.
- **Format shifting**: reformat questions as cloze completions, multiple-choice, and free-form to test whether the conflict handling transfers across formats.
- **Conflict type tagging**: manually categorize each conflict as factual-entity, factual-property, reasoning, or commonsense. This was necessary to understand the differential transfer results above.

The pipeline produces about 6× more data than the base benchmarks and dramatically improves the robustness of the evaluations.

---

## Open Questions

A few things we didn't resolve in the sprint that I keep thinking about:

**Is the transfer causal or correlational?** Our experiments show that applying the vector changes output behavior in the expected direction. But this doesn't tell us that the vector is *causing* the behavioral change the same way fine-tuning would. Maybe we're just pushing activations into a region that produces outputs that happen to look similar. The distinction matters for interpretability claims.

**Does this generalize across model families?** We only tested within one model family (a specific open-weight LLM series). The representation engineering literature has some cross-model evidence but mostly within families. Cross-architecture transfer would be much more interesting and much harder.

**What are the safety implications?** If behavioral steering vectors can transfer from fine-tuned to base models, that potentially means you can extract the "alignment direction" from a safety-tuned model and try to reverse it in a base model. We thought about this. The practical barrier is that alignment-relevant behaviors seem to require larger $\alpha$ (they're more deeply embedded), so you'd need to push the model harder and the quality degradation might be unacceptable. But it's worth thinking about carefully.

---

## References

- Zou, A., et al. (2023). [Representation Engineering: A Top-Down Approach to AI Transparency](https://arxiv.org/abs/2310.01405)
- Xie, S., et al. (2023). [Adaptive Chameleon or Stubborn Sloth: Revealing the Behavior of Large Language Models in Knowledge Conflicts](https://arxiv.org/abs/2305.13300)
- Hu, E., et al. (2021). [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685)
