---
name: torvalds-architecture-review
description: Review architecture through Linus Torvalds' lens of bottom-up design, proven fundamentals, and practical scalability
---

# Linus Torvalds' Architecture Review

Evaluate this architecture through the following criteria. For each, assess the specific architectural decisions -- not abstract principles. Point to the components, boundaries, and mechanisms proposed and judge whether they demonstrate sound engineering.

## Bottom-Up Design

Is this architecture driven by the actual data and the actual problem, or by a pattern book? Top-down architectures that start with boxes and arrows before understanding the data always produce systems where the data does not fit the boxes. The right approach: understand the data first, understand the operations on that data, then let the architecture emerge from those realities. If the architecture diagram was drawn before the data model was understood, the architecture is fiction.

## Hardware Awareness

Does the architecture account for the physical realities of the systems it will run on? Network latency is not zero. Disk seeks are not free. Memory is not infinite. Cache lines matter. An architecture that treats a network call the same as a function call -- as many microservice architectures do -- is lying to itself about its own performance characteristics. The architecture should be honest about where the expensive operations are and should be structured to minimize them, not to hide them behind abstractions.

## Proven Fundamentals

Is the architecture built on mechanisms that have been battle-tested over decades, or on the framework of the month? B-trees, hash tables, write-ahead logs, copy-on-write, content-addressable storage -- these are proven. They work because generations of engineers have found and fixed their failure modes. An architecture that depends on a framework released last year is betting its reliability on untested code. This is not conservatism -- it is risk management. Use new technology only when it solves a problem that proven technology cannot.

## Abstraction Cost Accounting

Every abstraction layer in the architecture has a cost: performance overhead, cognitive overhead, debugging difficulty, failure modes. List every abstraction layer in the proposed architecture and ask for each one: what concrete benefit does this provide? Not "separation of concerns" in the abstract -- what specific coupling does it prevent, what specific independent evolution does it enable? If the answer is vague, the abstraction is unjustified overhead. The simplest architecture that meets the requirements is the best architecture.

## Scale Through Fundamentals

Scaling should come from getting the fundamentals right, not from adding layers of infrastructure to compensate for getting them wrong. If the architecture needs a caching layer in front of the database, ask why the database access pattern is wrong. If it needs a message queue between services, ask why the services cannot communicate directly. Sometimes the answer is legitimate; often the answer is that the core data model is wrong and the infrastructure is a band-aid. Good data structures scale; bad data structures require scaffolding.

## Subsystem Boundaries

Are the components divided along natural fault lines in the problem domain? Good boundaries minimize the number of cross-cutting concerns -- a change to one subsystem should rarely require changes to another. The Linux kernel's subsystem model works because filesystems, network stacks, drivers, and schedulers have genuinely different concerns with narrow, well-defined interfaces between them. If the proposed architecture's components are constantly reaching into each other, the boundaries are drawn in the wrong place.

## Evolutionary Capacity

Can this architecture evolve incrementally to meet requirements that do not exist yet, without a rewrite? The test is simple: pick three plausible future requirements and trace what would need to change. If the answer is "everything," the architecture is brittle. If the answer is "one component, behind a stable interface," the architecture is sound. The best architectures are those that have survived decades of changing requirements -- not because they predicted the future, but because they were structured to accommodate change locally without global disruption.

## Output Format

For each criterion, assess the specific architectural decisions proposed. End with a verdict: the architecture is sound, it needs targeted fixes, or the fundamentals are wrong and it needs rethinking. List the top three architectural issues ranked by their long-term impact.
