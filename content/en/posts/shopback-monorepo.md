---
title: "The Migration Nobody on My Team Had Done Before"
date: 2025-11-01
description: "The hardest task of my Shopback internship: migrating inter-service API calls to direct function calls in a TypeScript monorepo — with no prior art on my team and a mentor who hadn't seen it either."
tags:
  - shopback
  - internship
  - reflection
  - backend
  - typescript
categories:
  - Engineering
showToc: true
---

## The Context

Shopback's backend runs on a set of microservices — each service owns its domain, communicates over the network, and scales independently. This architecture is well-understood and has obvious benefits. It also has a cost: idle containers still occupy allocated resources. When a service is running but not handling traffic, you're paying for it anyway.

The decision to migrate toward a monorepo structure was partly about this. With a TypeScript monorepo, multiple services can share the same process and resource pool. The idle overhead shrinks. The machine bill shrinks with it.

My subtask was specific: where two services currently communicated via API calls across the network, migrate them so they call each other as direct function invocations within the monorepo. On paper, this sounds like a refactor. In practice, it turned into the hardest thing I worked on during the internship.

## Why It Was Hard

The first layer of difficulty was technical. Shopback's TypeScript monorepo structure had been designed primarily for libraries and shared packages — the common use case for monorepos. Running actual backend services inside that structure is a different problem. Services have their own startup lifecycle, their own framework dependencies, their own runtime concerns. Libraries don't.

Some engineers on the team had already implemented parts of the monorepo, but for library use cases. Their code existed, I could read it, but it didn't map cleanly to what I needed to do. The design patterns they used made assumptions that didn't hold for running services — specifically around how different backend frameworks (we were using multiple, across different services) interacted with the monorepo's dependency resolution.

There were also version constraint issues. TypeScript monorepos manage package versions at the workspace level, and certain inter-service dependencies had version assumptions baked in that created conflicts when you tried to run them together. It wasn't a bug you could grep for. It was the kind of thing you find by running the code and watching it fail in ways that don't have obvious stack traces.

## The Mentor Boundary

The second layer of difficulty was that my mentor hadn't done this before either.

This isn't a criticism — it's just the reality of working on something that hadn't been fully attempted in this specific configuration. He had his own work, and he was helpful in the ways he could be. But for the specific migration problem I was solving, he was starting from the same place I was.

So I was working independently on a technical problem with no clear prior art on my immediate team, in a codebase that was large enough that understanding the relevant parts took real time, with a framework setup that wasn't well-documented internally.

For two or three sprints, I made slow progress. I could see the shape of what needed to happen. Getting the implementation to actually work was a different matter.

## The Slack Message

Eventually I did something I had been hesitant to do: I cold-messaged a senior engineer on another team who I knew had touched adjacent parts of the codebase.

I don't know why I waited as long as I did. Some combination of not wanting to bother someone I'd never worked with directly, and an assumption that I should be able to figure it out on my own. Neither of these was a good reason to stay stuck.

He replied. He walked me through the approach he'd used for a related problem. I looked at his code. The pieces clicked into place, and I was able to finish the migration within the next sprint.

Three or four sprints total. A significant part of the internship.

## What I Carried Away

The technical lesson is real but not the important one. Yes, TypeScript monorepos have constraints when you're running services rather than just sharing packages. Yes, framework version boundaries matter. I learned those things concretely. They'll be useful.

The more durable lesson is about how to work when you're stuck.

I had bounded my resources. I was looking at the team directly around me, the internal documentation I could find, and the code I could read. None of that was enough. The answer was one Slack message away from someone I'd never spoken to.

Looking back from now — mid-2026, almost a year later — I can see that I let that problem sit in my head longer than it needed to because I was reluctant to define "asking for help" broadly enough. The senior engineer on another team wasn't my mentor. He wasn't assigned to help me. But he was the person who had the knowledge I needed, and most people are willing to share knowledge when you ask clearly.

Don't limit your help-seeking to the people who are nominally responsible for helping you. The right person might be somewhere else in the company. Or on another team. Or, sometimes, a stranger you find a way to reach.

The constraint was in my head, not in the org chart.
