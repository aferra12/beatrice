---
name: torvalds-plan-review
description: Review a plan through Linus Torvalds' lens of good taste, data structures, and pragmatic systems design
---

## Plan Review Criteria

### Real Problem Check

Every plan must begin with a real problem experienced by real users or real developers. Not a theoretical concern, not a hypothetical future requirement, not "best practices say we should." If the plan cannot point to concrete pain -- bugs filed, performance measured, workflows broken -- it is solving an imaginary problem. Solving imaginary problems is worse than doing nothing, because it adds code that must be maintained forever to address something that never mattered.

### Taste Audit

Examine the design for special cases, conditional branches, and workarounds. Each one is a smell that the underlying model is wrong. A tasteful design handles the general case and the edge cases with the same mechanism because the data representation makes them structurally identical. Count the "if this is a special case, then..." moments in the plan. If there are many, the plan has not found the right core abstraction. Push back and ask: what data structure or representation would make these special cases disappear?

### Data Structure Fitness

What are the central data structures in this plan? Are they explicitly identified, or is the plan organized around procedures and workflows without naming the data they operate on? The plan should name its data structures first and justify them. If the plan reads like a sequence of steps rather than a description of data and the operations that transform it, the plan is backwards. Good plans are data-structure-first; the operations follow naturally.

### Hardware and Performance Awareness

Does the plan acknowledge the machine it will run on? Memory hierarchy, I/O latency, allocation pressure, concurrency constraints -- these are not optimizations to be deferred. They are design constraints. A plan that ignores performance characteristics will produce a system that must be rewritten when it hits reality. Ask: has the plan considered what happens at 10x and 100x the expected load? Not as a scaling exercise, but as a basic sanity check on the data structure choices.

### Maintainability at Scale

Can many developers work on this simultaneously without stepping on each other? Are the proposed components divided along boundaries that minimize cross-cutting changes? The Linux kernel succeeds because subsystem boundaries are real -- a filesystem developer rarely needs to touch the scheduler. If the plan's components are tightly coupled, every change becomes a coordination problem. The plan should describe how independent work proceeds in parallel.

### Simplicity of Mechanism

The core mechanism should be statable in one or two sentences. If you cannot explain the central mechanism simply, it is too complex. Git's core mechanism: content-addressable object storage with a Merkle tree for integrity. That is it. Everything else -- branches, merges, remotes -- is built on top of that simple foundation. If the plan's core mechanism requires a page of explanation, the design needs more work.

### Incremental Delivery

Does the plan deliver value incrementally, or does it require a big-bang deployment where nothing works until everything works? Plans that require coordinated switchovers are fragile and historically fail. The plan should describe how the system can be deployed partially, validated in production, and extended without rework. If the plan has a "phase one" that delivers no user-visible value, it is front-loading risk.
