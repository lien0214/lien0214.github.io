---
title: "RAG Defense via Graph Theory"
description: "A training-free, black-box defense that detects poisoned passages in retrieval-augmented generation by looking for graph asymmetry."
date: 2026-03-01
period: "Mar 2026 – Jul 2026"
team: "AIS Lab · 3-person team · Co-first author"
category: "Research"
status: "Under review at ACL ARR (Oct 2026)"
stack: ["RAG", "AI Safety", "Graph Theory", "Black-Box Defense", "Training-Free"]
---

## The Problem

Retrieval-augmented generation (RAG) trusts whatever the retriever returns. An attacker who can plant passages in the corpus can steer the model's answers, which is RAG poisoning.

## Our Approach

A defense that needs no training and no access to model internals: it treats the retrieved passages as a graph and detects poisoning through asymmetry in that graph. Because it is training-free and black-box, it can sit in front of an existing RAG pipeline without changing the model.

I was a co-first author on this 4-month project with [AIS Lab](https://ntuaislab.github.io/). The paper is currently under review in the October 2026 cycle of ACL Rolling Review (ARR).
