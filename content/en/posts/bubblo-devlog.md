---
title: "Leading a Game Project in a Semester: What Agile Doesn't Prepare You For"
date: 2025-07-10
description: "Three months, six people, one 2D platformer. What I learned adapting corporate workflow to a student team — and why the retrospective felt wrong the whole time."
tags:
  - bubblo
  - game-dev
  - project-management
  - teamwork
  - unity
  - reflection
categories:
  - Engineering
showToc: true
cover:
  image: /images/projects/bubblo-cover.png
  alt: "Bubblo — 2D pixel platformer title screen"
  relative: false
  hidden: false
  hiddenInSingle: false
---

## The Setup

April 2025. Six people, one semester, and a goal: ship a complete 2D platformer in Unity.

The game is called Bubblo. You play as a bubble creature exploring a pixel-art Wonderland, rescuing villagers trapped in cages, and fighting off needle-type enemies — bees, jumping spiders, a unicorn that very much wants to pop you. The core mechanic is bubble physics: bouncing and floating through levels, using your softness as both a movement tool and a combat advantage.

I was the project lead. My job was to translate a loose set of ideas into an actual, shippable game — and to keep five other people moving in the same direction for three months without anyone quitting.

## What I Brought from Cmoney

A few months before this, I was wrapping up my internship at Cmoney as a backend engineer. Nine months of real agile: sprint planning, daily standups, sprint reviews, retrospectives. I'd sat in enough of those meetings to have a surface-level understanding of what they were for.

So going into Bubblo, I had a framework. We'd run two-week sprints. We'd have a proper task board. We'd do retrospectives. I knew the vocabulary.

What I didn't fully understand yet was that the vocabulary came with assumptions baked in — assumptions about how people structure their time, what "availability" means, and what a team's primary obligation is.

At a company, everyone's primary job is the project. The sprint is the container. Agile ceremonies work because they're designed around full-time focus.

At school, that's not true for anyone.

## Why the Ceremonies Didn't Work

Our team had lab obligations, other coursework, extracurriculars, internship applications. One person was juggling a research paper deadline midway through our sprint. Another had a part-time job. Everyone was a full person with a full life outside of Bubblo.

Running a proper sprint planning session meant asking people to block two hours they didn't have. Running retrospectives felt performative when the real blocker was simply that people were busy and felt guilty about it. Daily standups became anxiety triggers instead of sync mechanisms.

The rigid structure was creating friction instead of removing it.

## Designing Something That Actually Fit

So I stripped it down.

The task board stayed — that was the most valuable thing. A shared view of what existed, what was in progress, and what was blocked. It cost everyone zero ceremony to use asynchronously.

I replaced scheduled standups with async check-ins: a short message at the start of each work session, no format required, just "working on X today, blocked by Y if anything." Low pressure. Actually read.

The weekly sync became one meeting with a single agenda: **what's blocking someone right now that I can unblock in the next 30 minutes?** Not status updates. Not progress reports. Just blockers. It ran 20-30 minutes and ended when we ran out of blockers.

The underlying principle was: respect that everyone's primary obligation isn't this project, but make it easy to contribute when they do have time. Compact, not loose.

## The OOP Refactor Nobody Wanted (But We Did Anyway)

About six weeks in, we had a problem. The avatar behaviour code had grown organically and was becoming a tangle. Multiple people were touching it, and changes in one place were breaking things in another in ways that were hard to trace.

We ran a refactor. Mid-sprint. On a student timeline.

Before any game code was written, I had designed the component architecture around Observer, State Machine, and Command patterns — mostly on paper. The refactor was about actually enforcing them in the implementation. Making the state transitions explicit. Making the event system the single source of truth for cross-component communication.

It was the right call. After the refactor, adding a new enemy type or a new player state became a contained operation. The last third of the sprint was noticeably less chaotic than the first two-thirds.

The timing felt wrong, but the alternative — continuing to patch a tangled codebase — would have been worse. Some technical debt compounds fast enough that you have to pay it early.

## The AI Context

We used ChatGPT throughout the project. This was before any of the IDE agent tooling existed — no Copilot Agents, no integrated code generation beyond autocomplete. Mostly it was: describe a problem, get a sketch, adapt the sketch.

Looking back a year later, the tooling has changed completely. A project like this today would look different — faster iterations, less time on boilerplate, different kinds of mistakes. I'm curious what gets *harder* as the tools improve, not just what gets easier. Probably the architecture decisions — the ones that don't have a clear right answer and require the team to commit to a direction and live with it.

## What I Actually Learned

The game shipped. 600+ commits, five playable levels, full audio, 60fps WebGL. We were proud of it.

But the thing I carry forward isn't the Unity knowledge or even the design patterns — it's the calibration on workflow design.

Agile isn't a formula. It's a set of principles built on specific assumptions about context. When the context changes — and student teams are a very different context than a company — the principles still apply, but the ceremonies have to be redesigned from scratch. The question is always: what is this ritual actually trying to achieve, and is this the lightest way to achieve it?

The other thing: project management in a peer team is mostly emotional work. Keeping people unblocked isn't just about task coordination — it's about making sure nobody feels like their contribution doesn't matter, or that the gap between "what I said I'd do" and "what I actually did" is going to be held against them. That kind of safety is what lets people show up when they have time, instead of avoiding the project because it feels like a source of guilt.

I didn't fully understand that going in. I do now.
