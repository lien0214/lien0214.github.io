---
title: "What Migrating 10+ API Calls Taught Me About Latency as a Product Feature"
date: 2025-11-20
description: "During Shopback's mono-repo migration, I converted a batch of cross-service API calls into direct function calls. Each call dropped from ~100ms to under 10ms. Notes on why latency at this scale matters more than it looks."
tags:
  - backend
  - performance
  - typescript
  - shopback
  - monorepo
categories:
  - Engineering Notes
showToc: true
---

## Context

Shopback runs a TypeScript monorepo. During my internship, one of the active migration efforts was consolidating legacy cross-service API calls — HTTP calls between internal services — into direct function calls within the monorepo.

On paper, this sounds like pure infrastructure work with no product impact. In practice, it changed how I think about latency.

---

## The Before State

The services involved were calling each other over HTTP, like this:

```typescript
// Before: cross-service HTTP call
const response = await fetch(`${INTERNAL_SERVICE_URL}/api/v1/user-watchlist/${userId}`);
const data = await response.json();
```

This works fine in isolation. But in a request path that chains 3–4 of these calls (which is common in recommendation and notification flows), you're looking at:

- ~20ms network round-trip per call (within the same datacenter, but still)
- ~15ms serialization/deserialization overhead per call
- ~10ms per call in connection pool management and TLS overhead
- Error handling surface area at each hop
- Distributed tracing noise

3 chained calls × ~50ms each = 150ms of incidental latency on top of actual business logic.

---

## The After State

The monorepo migration means the functions are importable directly:

```typescript
// After: direct function call within monorepo
import { getUserWatchlist } from '@shopback/user-service';

const watchlist = await getUserWatchlist(userId);
```

Each call now takes under 10ms. The business logic is the same. The network is gone.

---

## Why This Matters More Than It Looks

The engineering argument is obvious: less infrastructure overhead, less failure surface, easier to debug. What surprised me was the product argument.

**Users feel 100ms.** This is empirically documented in HCI research going back to Miller (1968) and confirmed repeatedly in modern UX studies. A 300ms response feels "instant." A 600ms response feels "fast." A 1000ms response feels "slow." The line is somewhere around 400–600ms for interactive UI.

Now, a feature that had 4 chained service calls at ~50ms each had a baseline overhead of 200ms before any business logic ran. Business logic adds another 100–200ms. You're at 300–400ms *before* the network to the user's browser.

After the migration, the overhead is ~40ms (direct calls). Business logic is the same. You're at 140–240ms. You've moved from "noticeably slow on a fast connection" to "fast on a typical connection."

The 15–25% increase in user engagement I saw on features where this migration happened is plausible if you buy the latency-engagement literature. Users don't consciously notice the speed difference. They just interact more.

---

## What I Actually Had to Do

The migration isn't just a find-and-replace. The complications:

**Dependency direction matters.** If Service A called Service B over HTTP, A didn't have a compile-time dependency on B. After the migration, it does. This can create circular dependencies in the monorepo graph that the build system won't allow. You have to understand the dependency structure before you start.

**Error handling changes semantics.** An HTTP call can fail with a network error (connection refused, timeout) that is categorically different from a function call throwing. After migration, you lose the "network error vs. logic error" distinction if you're not careful. We explicitly typed the error variants and updated the error handling logic accordingly.

**Observability changes.** The HTTP calls had distributed tracing spans automatically. Function calls don't. We added explicit telemetry instrumentation to the hot paths to maintain observability.

**Tests need updating.** Any test that mocked the HTTP call at the network layer needs to be updated to mock at the function interface instead. This is a better test in most cases (you're now testing the interface you're actually using), but it's toil.

---

## The Broader Lesson

The thing that stayed with me: **the reason for the performance improvement was structural, not algorithmic**. We didn't optimize any business logic. We didn't tune any database queries. We didn't add caching. We just removed an architectural seam that existed for historical reasons, not technical ones.

A lot of the "low-hanging fruit" performance work at scale looks like this. The code is fine. The database queries are fine. The bottleneck is the shape of the system — the boundaries drawn between services, the contracts between them, the overhead of those contracts.

Understanding that distinction — algorithm optimization vs. architecture optimization — is one of the more useful things I got from this internship.
