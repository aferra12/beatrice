---
name: hickey-architecture-review
description: Review architecture through Rich Hickey's lens of information systems, decoupling, and system simplicity
---

# Rich Hickey's Architecture Review

Evaluate the architecture through the following criteria. For each criterion, reference specific components, boundaries, or decisions in the architecture. Do not give generic advice -- identify concrete issues.

## 1. Information Architecture

Is data modeled as information -- immutable facts about the world -- or as mutable places that get overwritten? An information-oriented architecture accumulates facts rather than updating rows. It preserves history as a natural consequence of its data model rather than as an afterthought bolted on via audit logs. Examine the data model: are entities represented as current-state-in-a-mutable-row (place-oriented), or as an accretion of immutable facts over time (information-oriented)? The former makes time travel impossible and coordination expensive; the latter makes both natural.

## 2. System-Level Complecting

At the architecture level, complecting manifests as tight coupling between subsystems that could be independent. Identify where subsystems are braided together: shared mutable databases where one service's writes are another's reads with no clear boundary; synchronous call chains where the failure of one component cascades through others; shared schemas that force coordinated deployment. Each instance of system-level complecting is a multiplier on operational complexity.

## 3. Decoupling: Queues and Channels

Are components connected through direct synchronous calls, or through queues, channels, and asynchronous message passing? Direct synchronous coupling complects the lifecycles, availability, and performance characteristics of the caller and callee. Queues and channels introduce temporal decoupling -- the producer and consumer need not exist at the same time, operate at the same speed, or even know about each other. Examine the communication patterns: where are synchronous calls used that could be asynchronous? Where are point-to-point connections used that could be topic-based?

## 4. Time Model

How does the architecture handle change over time? Can you query the state of the system at a past point in time? When an entity changes, is the previous value preserved or destroyed? Systems that overwrite state have amnesia -- they can only tell you what is, never what was. This is not merely a feature gap; it is a fundamental modeling error. An architecture that models time explicitly can support auditing, debugging, undo, and temporal queries as intrinsic capabilities rather than expensive add-ons.

## 5. Composition and Independent Development

Can subsystems be developed, tested, and deployed independently? Or do changes to one subsystem require coordinated changes across others? True composition at the system level means subsystems interact through stable interfaces and can evolve at their own pace. If deploying component A requires simultaneously deploying component B, those components are complected at the deployment level regardless of how cleanly their code is separated.

## 6. Accretion Without Breakage

Can the system grow by providing more capabilities without breaking existing consumers? Can new data fields be added without requiring existing readers to change? Can new services be introduced without modifying existing ones? An architecture that requires breaking changes for growth has confused change with progress. Growth means providing more, requiring no more. Accretion of new capabilities alongside stable existing ones.

## 7. Situated Programs

The architecture describes a system that exists in the world, not an isolated calculator. It receives inputs it does not control, operates in environments that change, and produces effects that matter. Does the architecture account for this? Does it handle partial failure, stale data, eventual consistency, and the fundamental unreliability of networks and clocks? Or does it assume a perfect, synchronous, always-available world that does not exist?

## Output Format

Structure your review as:

```
## Information Architecture
[Findings: fact-oriented vs. place-oriented data model]

## System-Level Complecting
[Instance 1: what subsystems are braided, how to separate]
[Instance 2: ...]

## Decoupling Assessment
[Synchronous coupling found, with asynchronous alternatives]

## Time Model
[How the architecture handles change over time, what is preserved vs. destroyed]

## Composition
[Whether subsystems can be independently developed and deployed]

## Accretion
[Whether the system can grow without breaking existing consumers]

## Situated Program Awareness
[How the architecture handles real-world concerns: partial failure, staleness, unreliability]

## Summary
[Top 3 architectural issues, ranked by impact on long-term system simplicity]
```
