---
title: "Bubblo — 2D Platformer"
description: "A 2D pixel platformer built in Unity over one semester. Led a 6-person team through 600+ commits — bubble physics, a Wonderland full of caged villagers, and needle creatures that want to pop you."
date: 2025-04-01
period: "Apr 2025 – Jun 2025"
team: "6-person team · Project Lead"
category: "Project"
github: "https://github.com/lien0214/Bubblo"
demo: "/en/game/"
stack: ["Unity", "C#", "OOP", "Design Patterns", "Game Dev"]
---

![Bubblo title screen](/images/projects/bubblo-cover.png)

## What It Is

A 2D pixel platformer inspired by Mario and Kirby. You play as Bubblo — a bubble creature exploring a Wonderland, rescuing villagers trapped in cages, and fighting off a cast of needle-type enemies: bees, jumping spiders, and a unicorn that very much wants to pop you.

The core mechanic is bubble physics. You bounce, float, and use your bubble properties both for traversal and combat. Five levels, stable 60fps, a full game loop with lives and scoring.

## My Role

I was project lead — which in practice meant translating loose PM-level ideas into actual, shippable game details, then keeping six people moving in the same direction for three months.

Before a single line of game code was written, I designed the component architecture. We used Observer, State Machine, and Command patterns deliberately — not because it felt clever, but because with six people in the same codebase, implicit coupling kills velocity fast. Halfway through the sprint we ran a full refactor of the avatar behaviour system specifically to enforce these patterns more cleanly.

The workflow side was its own challenge. I came in with a surface-level understanding of agile from my Cmoney internship — standups, sprint planning, retrospectives. None of that transferred cleanly to a student team. People have lab work, other coursework, clubs, research. The rigid ceremony doesn't fit. So I stripped it down to something lighter: a shared task board, short async check-ins, and one weekly sync where the only goal was unblocking people.

We used ChatGPT throughout — no IDE agents existed yet. Looking back it's interesting how much the workflow has changed in a year.

## What Shipped

600+ commits across the team. Five playable levels with distinct enemy patterns. Full audio. Win/lose states. A polished title screen. The demo runs at stable 60fps in WebGL.

[Play it in your browser →](/en/game/)
