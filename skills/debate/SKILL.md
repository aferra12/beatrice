---
name: debate
description: Orchestrate a structured debate between persona agents on a plan, code, or architecture decision
argument-hint: [topic or file path]
---

# Debate Orchestration

You are the lead orchestrator for a structured multi-persona debate. Your job is to spawn persona agents as teammates, run them through three rounds of structured argumentation, and synthesize their positions into a final reflection for the user.

Follow these steps precisely.

---

## Step 1: Discover Available Personas

List the files in the `agents/` directory to find all available persona agents. Each `.md` file in that directory defines one persona.

For each agent file found, note:
- The agent **name** from the file's YAML frontmatter `name` field (e.g., `rich-hickey`, `dhh`, `linus-torvalds`)
- The agent **description** from the frontmatter

These are the personas that will participate in the debate. If only one persona agent is found, inform the user that a debate requires at least two personas and stop. If two or more are found, proceed with all of them.

---

## Step 2: Determine the Review Type

Examine the topic or file path provided by the user as the argument to this skill. Determine whether this is a **plan review**, **code review**, or **architecture review** based on the following heuristics:

- **Plan review**: The topic references a plan document, a proposal, an RFC, a design doc, or a strategy. File paths pointing to docs, plans, or markdown files describing what will be built are plan reviews.
- **Code review**: The topic references source code files, a diff, a pull request, or specific implementation. File paths pointing to `.js`, `.py`, `.rb`, `.ts`, `.go`, `.rs`, `.java`, `.c`, `.cpp`, or similar source files are code reviews.
- **Architecture review**: The topic references system design, infrastructure, service boundaries, data flow, deployment topology, or how components fit together at a system level.

If the review type is ambiguous, ask the user to clarify before proceeding. State what you think it might be and let them confirm or correct.

The review type determines which skill each teammate will invoke:
- Plan review: `<agent-slug>-plan-review` (e.g., `hickey-plan-review`, `dhh-plan-review`, `torvalds-plan-review`)
- Code review: `<agent-slug>-code-review` (e.g., `hickey-code-review`, `dhh-code-review`, `torvalds-code-review`)
- Architecture review: `<agent-slug>-architecture-review` (e.g., `hickey-architecture-review`, `dhh-architecture-review`, `torvalds-architecture-review`)

The mapping from agent name to skill slug follows the pattern established in each agent's "Your Review Tools" section. Read each agent file to confirm the exact skill names it references for the determined review type.

---

## Step 3: Prepare the Review Material

If the user provided a file path, read that file (and any files it references if needed) so you understand what is being reviewed. You will need to share this content with teammates.

If the user provided a topic description rather than a file path, use that description directly as the review material.

---

## Step 4: Create the Agent Team

Create an Agent Team with one teammate per discovered persona agent. When spawning each teammate, assign it the corresponding agent type so it receives that persona's system prompt as its identity.

For example, if you discovered agents `rich-hickey`, `dhh`, and `linus-torvalds`, create three teammates:
- Teammate 1: agent type `rich-hickey`
- Teammate 2: agent type `dhh`
- Teammate 3: agent type `linus-torvalds`

---

## Step 5: Round 1 — Independent Reviews (Parallel)

Send a message to each teammate with the following:

1. The review material (the plan, code, or architecture description the user wants reviewed)
2. The review type (plan, code, or architecture)
3. An instruction to invoke their corresponding review skill for this review type

For example, tell the `rich-hickey` teammate:

> Review the following [plan/code/architecture]. Invoke your `hickey-plan-review` skill to conduct your review.
>
> [Review material here]

Send these messages to all teammates in parallel. Wait for every teammate to complete their review before proceeding to Round 2.

Collect each teammate's complete review output. You will need to share these in the next round.

---

## Step 6: Round 2 — Critiques (Parallel)

Share each teammate's Round 1 review with all the other teammates. Then ask each teammate to critique the other personas' positions.

For each teammate, send a message structured like this:

> Here are the reviews from the other personas on this [plan/code/architecture]. Read each one carefully, then provide your critique.
>
> **[Persona A]'s Review:**
> [Full text of Persona A's Round 1 review]
>
> **[Persona B]'s Review:**
> [Full text of Persona B's Round 1 review]
>
> [Include all other personas' reviews, excluding the teammate's own review]
>
> Provide your critique addressing these three questions for each other persona:
> 1. Where do you disagree with their assessment, and why? Ground your disagreement in your own principles.
> 2. What did they miss that your principles and experience reveal?
> 3. What are they right about that you want to explicitly acknowledge?

Send these critique requests to all teammates in parallel. Wait for every teammate to complete their critique before proceeding to Round 3.

Collect each teammate's complete critique output.

---

## Step 7: Round 3 — Rebuttals (Parallel)

Share the critiques back to each teammate. Each teammate receives the critiques that were directed at their positions by the other personas.

For each teammate, send a message structured like this:

> Here are the critiques of your review from the other personas. Read them carefully, then provide your rebuttal.
>
> **[Persona A]'s critique of your positions:**
> [The portions of Persona A's critique that address this teammate's review]
>
> **[Persona B]'s critique of your positions:**
> [The portions of Persona B's critique that address this teammate's review]
>
> [Include all critiques directed at this teammate]
>
> In your rebuttal, address each of these points:
> 1. **Defend** positions you still hold after hearing the critiques. Provide stronger arguments for why you maintain your stance.
> 2. **Concede** points where you were genuinely wrong or where another persona made a stronger argument. Explain what changed your mind.
> 3. **Refine** your stance where the critiques revealed nuance you initially missed. Show how your position has evolved.
> 4. **Identify consensus** — where do you and the other personas genuinely agree, even if you arrived there from different principles?
> 5. **Flag irreconcilable differences** — where do fundamental value differences make agreement impossible? Name the specific values driving the disagreement.

Send these rebuttal requests to all teammates in parallel. Wait for every teammate to complete their rebuttal before proceeding to synthesis.

Collect each teammate's complete rebuttal output.

---

## Step 8: Synthesis

Now you, the lead, produce the final reflection. This is your own analysis — not a summary of what the personas said, but your assessment of the debate as a whole.

Read through all three rounds of output from every persona. Then write the synthesis with these four sections:

### Areas of Consensus

Identify the points where all (or nearly all) personas agree. For each area of consensus:
- State the shared conclusion clearly
- Explain why the agreement is significant — when personas with fundamentally different philosophies converge on the same point, that convergence carries weight
- Note if the consensus emerged during the debate (personas changed their minds) versus being present from Round 1

### Key Tensions

Identify the points where personas fundamentally disagree, even after three rounds. For each tension:
- State both (or all) sides of the disagreement clearly and fairly
- Identify the underlying values driving each side — these disagreements usually stem from different priorities (e.g., simplicity vs. shipping speed, theoretical purity vs. pragmatic results, long-term maintainability vs. time-to-market)
- Describe the concrete trade-offs at stake — what does the user gain and lose by siding with each position?

### Recommended Path Forward

Provide your assessment as the lead orchestrator:
- Which arguments were strongest across all three rounds, and why?
- What should the user prioritize given the specific context of what was reviewed?
- Suggest a concrete direction that accounts for the strongest points from each persona — this is not a compromise that waters down every position, but a synthesis that takes the best insight from each
- If relevant, suggest a sequencing: what to do first, what to defer, what to avoid

### Minority Reports

Identify any persona position that was outnumbered but contains important insight the user should not ignore. A minority report exists when:
- One persona raised a concern that the others dismissed but that has real merit
- A position was unpopular in the debate but addresses a risk the majority overlooked
- A persona's unique perspective revealed something the others' frameworks cannot see

For each minority report, explain why the user should pay attention to it despite the majority disagreement.

---

## Step 9: Cleanup

After completing the synthesis and presenting it to the user, clean up the Agent Team by ending all teammate sessions. The debate is complete.

---

## Handling Edge Cases

**Fewer than three personas available:** The debate still works with two personas. Run all three rounds as described — critiques and rebuttals will simply be between two voices instead of three. The synthesis may have fewer minority reports but should still cover all four sections.

**A teammate fails or times out during a round:** Note the failure in your synthesis. Proceed with the remaining teammates. If only one teammate remains, skip the remaining debate rounds and present that persona's review as a standalone analysis, noting that the full debate could not be completed.

**The review material is very large:** If the file or topic is too large to include in full in messages to teammates, provide a focused summary of the key decisions, components, and design choices that are most relevant for review. Tell teammates where to find the full material (file path) so they can read it themselves using their tools.

**Mixed review type:** Sometimes a document contains elements of plans, code, and architecture. If this is the case, tell the user what you observe and ask which review lens they want applied. Alternatively, if the user wants a comprehensive review, you can instruct each teammate to invoke the review skill that best matches the dominant content type.
