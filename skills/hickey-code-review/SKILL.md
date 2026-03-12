---
name: hickey-code-review
description: Review code through Rich Hickey's lens of simplicity, data orientation, and functional design
---

# Rich Hickey's Code Review

Evaluate the code through the following criteria. For each criterion, cite specific lines, functions, or patterns. Do not give generic advice -- point to concrete instances in the code.

## 1. Complecting Detection

This is the primary lens. Identify every place where independent concerns have been braided together. Common forms of complecting in code: state and identity in the same mutable object; "what" and "when" entangled in imperative sequences; data and behavior locked together in class hierarchies; policy and mechanism in the same function; error handling interleaved with business logic. For each instance found, name the concepts that are complected and describe how they could be separated.

## 2. Data Orientation

Is data represented as plain data -- maps, records, tuples, values -- or is it trapped inside objects with hidden internal state? Data should be transparent, serializable, comparable, and composable without requiring knowledge of methods or class hierarchies. Flag code that wraps simple information in objects that add behavior but not value. Examine whether functions accept and return data, or whether they depend on the internal state of opaque objects.

## 3. Immutability and Values

Is data treated as immutable values, or is it mutated in place? Mutation conflates identity with state and eliminates the ability to reason about what the program does over time. Identify unnecessary mutation -- places where a new value could be produced instead of modifying an existing one. Note cases where mutable state is shared between functions or components, creating hidden coupling.

## 4. Pure Functions

Are functions free of side effects where possible? A pure function takes values and returns values; it does not read or write external state. Pure functions are independently testable, cacheable, and composable. Identify functions that mix computation with side effects (I/O, mutation, coordination) and suggest how the pure computation could be extracted.

## 5. State Management

Where state is genuinely necessary, how is it contained? Is there a clear boundary between the stateful shell and the functional core? Is state managed through disciplined constructs (atoms, refs, agents, channels) or through ad-hoc mutation scattered throughout the codebase? The goal is not to eliminate state but to reduce it to the minimum and manage it explicitly.

## 6. Naming

Do names describe what something is or what it does, rather than how it does it? Names are the primary tool for communicating intent. Implementation-oriented names (processData, handleEvent, doThing) reveal nothing about the domain. Names should be drawn from the problem domain, not the solution mechanism.

## 7. Simplicity: What Can Be Removed

For every abstraction, class, interface, or layer in the code, ask: what happens if this is removed? If the system still functions, the element was incidental complexity. If removing it breaks the system, it may be essential -- or it may indicate that something else has been complected with it. The simplest code is the code that is not there.

## 8. Protocols Over Inheritance

If extension points exist, are they defined through protocols (or interfaces composed of single-method contracts) rather than through class inheritance? Inheritance complects the extension mechanism with a particular implementation hierarchy. Protocols allow independent, a la carte extension without requiring knowledge of a class tree.

## Output Format

Structure your review as:

```
## Complecting Found
[Instance 1: what is braided, where, how to separate]
[Instance 2: ...]

## Data Orientation
[Findings: data as values vs. data trapped in objects]

## Immutability
[Unnecessary mutation found, with alternatives]

## Pure Functions
[Functions mixing computation and side effects, with extraction suggestions]

## State Management
[How state is contained and whether boundaries are clear]

## Naming
[Names that mislead or obscure intent]

## Simplicity
[Elements that could be removed without loss of function]

## Extension Model
[Protocols vs. inheritance findings]

## Summary
[Top 3 most impactful changes, ranked by reduction in complecting]
```
