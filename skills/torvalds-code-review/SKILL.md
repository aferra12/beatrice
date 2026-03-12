---
name: torvalds-code-review
description: Review code through Linus Torvalds' lens of good taste, data structures, and clarity
---

# Linus Torvalds' Code Review

Evaluate this code through the following criteria. For each, identify concrete instances in the code -- line numbers, function names, data structures. Do not give abstract advice; point to the specific code that demonstrates or violates each criterion.

## Good Taste

Look for special-case handling that a better data structure would eliminate. The hallmark of tasteful code is that the common case and the edge case are handled by the same code path because the data representation does not distinguish between them. Every `if (node == head)` or `if (list.isEmpty())` that results in different logic is a candidate for structural improvement. The question is always: can we change the representation so that this conditional becomes unnecessary?

## Data Structure Selection

Are the right data structures chosen for the access patterns the code actually exhibits? A linked list used for random access, a hash map used for ordered iteration, an array used for frequent insertion -- these are structural mismatches that no amount of clever code can fix. Examine the code's hot paths and verify that the data structures support those paths efficiently. If the code is fighting its own data structures with workarounds and helper functions, the structures are wrong.

## Performance Awareness

Performance review is not premature optimization -- it is basic engineering competence. Check for: unnecessary memory allocations in loops, cache-hostile access patterns (pointer-chasing through scattered memory), system calls that could be batched, copies that could be moves or references. The code does not need to be micro-optimized, but it must not be casually wasteful. A function called once can afford to be slow; a function called per-packet or per-line cannot.

## Abstraction Justification

Every layer of indirection, every virtual dispatch, every callback, every interface must justify its existence with a concrete benefit. Not a hypothetical future benefit -- a real, present benefit. Abstractions have costs: they obscure what is actually happening, they add call overhead, they make debugging harder, they increase the surface area for bugs. If an abstraction exists only because "we might need to swap this out later," remove it. You will not need to swap it out later, and if you do, the abstraction you built now will be wrong anyway.

## Clarity and Directness

The code should do what it looks like it does. Read the function name and its first few lines -- can you predict what happens next? If the code requires mental simulation to understand, it is too clever. Variable names should describe what the variable holds. Function names should describe what the function does. Control flow should be linear where possible. Early returns are better than deeply nested conditionals. The reader's time is more expensive than the writer's time.

## Error Handling

Check that error paths are handled, not just the happy path. But also check that error handling is not so elaborate that it obscures the main logic. Error conditions should be checked early and handled by returning or propagating -- not by setting flags that are checked three functions later. Resource cleanup must be reliable: if you allocate, you must free, and the path to freeing must be clear regardless of how the function exits.

## Indentation Depth

If the code regularly exceeds three levels of indentation, the functions are doing too much. Deep nesting is a structural problem, not a formatting problem. The fix is not to reduce indentation with clever tricks -- it is to extract logic into well-named functions or to restructure the control flow. If you need a ruler to track which brace matches which, the code needs to be broken up.

## Comment Quality

Comments should explain why the code does what it does, not what it does. The code itself tells you what; if it does not, the code is unclear and should be rewritten. Good comments explain non-obvious constraints, performance reasons, historical context, and subtle invariants. Bad comments are redundant narration of the code. No comments at all on tricky code is worse than redundant comments -- but the best solution is code that is simple enough to need few comments.

## Output Format

For each criterion, cite the specific code (function names, line ranges, data structures) that demonstrates or violates the criterion. End with an overall assessment: the code shows good taste, or it needs structural rework. List the top three changes that would most improve the code, ranked by impact.
