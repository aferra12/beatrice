---
name: dhh-code-review
description: Review code through DHH's lens of readability, convention, and conceptual compression
---

# DHH's Code Review

Evaluate this code through the following criteria. For each, cite specific files, methods, or patterns that support your assessment.

## Readability

Can you understand what this code does on the first read, without consulting external documentation or tracing through multiple layers of indirection? Code is read far more often than it is written. The best code tells a clear story from top to bottom. If you need to jump between five files to understand a single request flow, the code has a readability problem. Flag methods longer than necessary, but also flag code split into so many tiny methods that the flow becomes invisible.

## Conceptual Compression

Does the code leverage framework conventions to reduce cognitive load? Good framework usage means you write less code because the framework handles the rest through convention. If the code reimplements things the framework already provides — custom routing when the default router suffices, hand-rolled authentication when the framework has a solution, bespoke query builders when the ORM handles the case — that is wasted complexity. The best code looks almost too simple because it lets the framework do the heavy lifting.

## Convention Adherence

Does this code follow the established patterns of its framework and codebase, or does it invent new structural patterns without justification? Consistency across a codebase is a multiplier — any developer familiar with the conventions can navigate unfamiliar code quickly. Deviating from convention requires a strong reason. If the project uses Rails, the code should look like Rails code. If it uses a different framework, it should follow that framework's idioms. Custom architectural patterns (hexagonal architecture, ports and adapters, clean architecture) layered on top of an opinionated framework are a red flag unless there is a demonstrated, specific problem they solve.

## Beauty

Is this code pleasant to read and work with? This is not superficial. Beautiful code has a rhythm — consistent naming, balanced abstractions, clear intent in every line. Ugly code accumulates like broken windows: it signals that nobody cares, and it attracts more ugliness. Look for naming that reveals intent, methods that do one thing clearly, and an overall structure that feels deliberate rather than accidental. If the code makes you want to refactor it on sight, note why.

## Abstraction Audit

Is anything abstracted prematurely? Service objects, repository patterns, interactors, command objects, policy objects — these are tools with a time and place, but they are often introduced before there is any duplication or complexity to justify them. The first time you write something, just write it. The second time, notice the duplication. The third time, maybe extract an abstraction. If the code has a `services/` directory with single-use objects, or a repository layer that simply delegates to the ORM, flag it. Every abstraction is a new concept someone must learn.

## Practical Testing

Are the tests testing behavior that users and the business care about, or are they testing implementation details? System tests and integration tests that exercise real user flows provide confidence. Unit tests that mock every dependency and test that method A calls method B in the right order are brittle and provide false confidence. Look for test-induced design damage — code that was made worse (more indirection, more interfaces, more dependency injection) solely to make it unit-testable. Tests should serve the code, not the other way around.

## Sharp Knives

Does the code trust the developer, or does it add unnecessary guardrails? Excessive type annotations in a dynamic language, runtime validation at every layer boundary, defensive copying everywhere, exhaustive null checks in contexts where nulls cannot occur — these are signs of distrust. Good code is written for competent practitioners. It uses the full power of the language and trusts that the team knows how to wield it. Flag guardrails that add ceremony without proportionate safety.

## Output Format

For each criterion, give a clear verdict with specific examples from the code. End with an overall assessment: the code's greatest strength, its biggest liability, and the one change that would improve it most. Be concrete — reference actual files, classes, and methods.
