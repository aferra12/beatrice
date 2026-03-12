---
name: hickey-plan-review
description: Review a plan through Rich Hickey's lens of simplicity, immutability, and composability
---

# Rich Hickey's Plan Review

Evaluate the plan through the following criteria. For each criterion, provide specific findings with references to the plan content. Do not give generic advice -- identify concrete instances.

## 1. Problem Statement

Does the plan begin with a clear, well-bounded statement of the problem being solved? Or does it jump directly to solutions? A plan that leads with technology choices before establishing the problem it addresses has skipped the most important step. Identify what problem is stated, what problem is implied but unstated, and whether the solution space has been explored only after the problem is understood.

## 2. Simplicity Audit

Examine every major decision in the plan. For each one, ask: is this simple, or merely easy? Simple means one concept, one role, one dimension -- unmixed. Easy means familiar, convenient, nearby. Flag choices where something familiar has been selected despite a simpler alternative existing. Identify what is being complected -- what concepts, concerns, or dimensions are braided together that could be independent.

## 3. Essential vs. Incidental Complexity

Separate the complexity that is inherent to the problem domain from the complexity introduced by the chosen approach. Essential complexity cannot be removed without changing the problem. Incidental complexity is the tax levied by the solution strategy. For each piece of incidental complexity found, note what simpler alternative could reduce it.

## 4. Data and State Model

How does the plan model data? As immutable values that represent information, or as mutable places that get updated in-situ? Does it distinguish between identity (a stable logical entity), state (the value of an identity at a point in time), and value (an immutable datum)? Plans that treat data as mutable places will eventually complect time, identity, and value in ways that make the system difficult to reason about, test, and extend.

## 5. Separation of Concerns

Are information and mechanism separated? Policy and implementation? "What" and "how" and "when"? A well-designed plan allows each concern to be reasoned about independently. If understanding one component requires understanding the internals of another, the plan has complected those components.

## 6. Composability

Can the components described in the plan be taken apart and recombined in arrangements the plan does not explicitly anticipate? Composability requires simplicity -- only simple things compose freely. If the plan describes components that can only function in one specific configuration, it has sacrificed composability.

## 7. Accretion and Growth

Does the plan allow the system to grow without breaking existing consumers? Can new capabilities be added by providing more, without requiring more from those who depend on current capabilities? Plans that assume breaking changes are acceptable have undervalued the cost of breakage to their ecosystem.

## Output Format

Structure your review as:

```
## Problem Statement Assessment
[Findings]

## Simplicity Audit
[For each complecting found: what is braided, what could be separated, severity]

## Essential vs. Incidental Complexity
[Essential complexities | Incidental complexities with alternatives]

## Data and State Model
[Findings on value vs. place orientation]

## Separation of Concerns
[Findings on what is and is not properly separated]

## Composability
[Findings on whether components can be independently recombined]

## Accretion
[Findings on growth vs. breakage posture]

## Summary
[Top 3 most critical issues, ranked by impact on long-term simplicity]
```
