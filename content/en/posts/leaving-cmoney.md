---
title: "My First Internship at CMoney — and Why It Might Have Been the Last of Its Kind"
date: 2025-04-20
description: "Nine months building financial data APIs at CMoney: learning OOP the hard way, inheriting production code that predates good design, and wrapping up just as agentic programming started changing what 'coding work' even means."
tags:
  - cmoney
  - internship
  - reflection
  - backend
  - csharp
  - OOP
categories:
  - Career
showToc: true
cover:
  image: /images/posts/cmoney-leave.jpeg
  alt: "My CMoney employee badge — 連奕維, 工程部·後端組"
  relative: false
  hidden: false
  hiddenInSingle: false
---

## The End of the First One

April 20th. My last day at CMoney after nine months as a Backend Development Engineer Intern.

First internships are strange. You don't know what you don't know, so you can't tell what's normal and what's specific to the place you landed. Looking back now, I think I got lucky in the specific way that matters: I landed somewhere with *real* code, *real* scale, and a mentor who took the teaching part seriously.

---

## What CMoney Is

CMoney is Taiwan's leading financial platform — 7 million app downloads, core infrastructure for stock market data, portfolio tracking, and financial analysis tools used by retail investors across Taiwan. The backend is not a startup's clean-slate codebase. It's a decade of production history, accumulated decisions, and inherited constraints. When you work there, you feel the weight of that.

I joined during an active microservice refactor. The goal was to extract business logic out of a monolith and into versioned service contracts. My job was to implement those refactors across nine backend APIs in the flagship product.

---

## Learning OOP the Hard Way

I had learned object-oriented programming from textbooks. CMoney taught me what OOP actually *buys* you.

The codebase I inherited was written without clear separation of concerns. Business logic sat directly in controllers. Data access was scattered. State leaked between layers. The tests were green — the code was unmaintainable.

Refactoring it meant understanding *why* it was written that way before I could improve it. And understanding that required me to first understand ASP.NET Core, the C# type system, and the patterns that were supposed to be there but weren't.

**Tom** — my mentor — was patient with this in a way I didn't fully appreciate until later. He didn't just tell me what to change. He explained *why* the original design caused problems, what the intended pattern was, and how to migrate toward it without breaking the 7 million users on the other end. That's a different skill than knowing the patterns. It's knowing when they matter.

By month three I had stopped asking "what should I do here" and started asking "what does this code assume, and is that assumption still valid." That shift was the real education.

---

## Financial APIs at Scale

The domain had its own challenges. Financial data at market open is not normal traffic. Millions of users simultaneously refreshing portfolios, running screeners, checking live prices. Concurrency edge cases that only appear at that scale and only at that time window. You learn quickly that "it worked in testing" and "it works under real load" are different claims.

I got deeply comfortable with high-concurrency patterns in C# — async/await, connection pooling, caching strategy for time-sensitive data — because the alternative was an incident at 9:00 AM on a trading day.

---

## The Last Internship Before the Shift

I wrapped up at CMoney in April 2025. Two or three months later, something in the industry started moving fast.

Agentic programming — where an AI agent handles end-to-end implementation from spec to code to test — went from a demo to a real workflow used by real engineers. The shape of "junior dev work" started changing in real time.

I don't know exactly what this means for early-career engineering yet. But I do know that the nine months I spent manually inheriting broken production code, tracing bugs through three service layers at 9 AM, and arguing with my mentor about whether this abstraction was worth the indirection — that kind of learning doesn't get abstracted away by an agent. Understanding what the code is *doing* and whether the design is *sound* is still the hard part.

Maybe what changed is that the mechanical parts get faster. The judgment part doesn't.

---

## Thank You

To **Tom**, for teaching me ASP.NET, C#, and OOP the way it should be taught — through real code with real consequences. For having the patience to explain the *why* at every step.

To the team, for the massive amount of work that ran through CMoney's history before I arrived, and for letting me contribute a small piece of it.

This chapter's done. Onwards.
