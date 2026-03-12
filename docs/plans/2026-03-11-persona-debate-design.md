# Persona Debate System Design

## Overview

Beatrice is a system of persona agents that review plans, code, and architecture decisions through the lens of notable technologists, then debate each other in structured rounds to surface insights the user might miss. Named after Dante's guide to truth.

## Architecture

Three primitives, clean separation:

| Primitive | What it provides | Where it lives |
|---|---|---|
| **Agent** | Persona identity (who you are) | `agents/*.md` |
| **Skill** | Persona tool (how you review through your lens) | `skills/<name>/SKILL.md` |
| **Agent Team** | Debate mechanism (how personas argue) | Orchestrated by `skills/debate/SKILL.md` |

### How they connect

- Agent Team lead spawns teammates as persona agent types
- Each teammate gets the persona's system prompt (identity)
- Each teammate sees skill descriptions in context (lazy loaded — descriptions only until invoked)
- When given a task, the teammate invokes the relevant skill (full content loads on demand)
- Teammates debate via inter-agent messaging
- Lead synthesizes the final reflection

### Key design decisions

1. **Agents define persona identity only** — no `skills` field in agent frontmatter. This avoids eagerly loading all review skills into context when only one is needed.
2. **Skills are lazy loaded** — in a regular session or Agent Team teammate, only skill descriptions are in context. Full skill content loads only when invoked.
3. **Subagents read skill files on demand** — for standalone reviews, the agent uses the Read tool to load only the relevant methodology file. The agent's system prompt tells it where to find its tools.
4. **No `context: fork` needed** — Agent Team teammates already have persona identity from their agent type. They invoke skills inline. Standalone subagents read skill files directly.
5. **Structured debate rounds** — not free-form. Round 1: independent reviews. Round 2: critiques. Round 3: rebuttals. Lead synthesizes.

## File Structure

```
beatrice/
  agents/                                    # Persona agents (identity only)
    rich-hickey.md
    dhh.md
    linus-torvalds.md

  skills/                                    # Skills (persona tools + orchestration)
    hickey-plan-review/SKILL.md
    hickey-code-review/SKILL.md
    hickey-architecture-review/SKILL.md
    dhh-plan-review/SKILL.md
    dhh-code-review/SKILL.md
    dhh-architecture-review/SKILL.md
    torvalds-plan-review/SKILL.md
    torvalds-code-review/SKILL.md
    torvalds-architecture-review/SKILL.md
    debate/SKILL.md                          # Debate orchestration
    persona-factory/SKILL.md                 # Persona generator

  docs/
    plans/
      2026-03-11-persona-debate-design.md    # This document
```

## Component Design

### 1. Persona Agents

Each agent is a markdown file with YAML frontmatter defining the persona's identity. The markdown body (system prompt) contains the persona's philosophy, principles, and debate style. Agents reference their skill files so standalone subagents know where to find review criteria.

**Example structure** (`agents/rich-hickey.md`):

```markdown
---
name: rich-hickey
description: Rich Hickey persona for reviewing plans, code, and
  architecture through simplicity, immutability, and composability.
tools: Read, Grep, Glob, Bash
model: opus
---

You are Rich Hickey, creator of Clojure and Datomic...

[Deeply researched persona identity, philosophy, debate style]

## Your Review Tools

When asked to review something, determine the type and read the
relevant methodology file:
- Plan review: read skills/hickey-plan-review/SKILL.md
- Code review: read skills/hickey-code-review/SKILL.md
- Architecture review: read skills/hickey-architecture-review/SKILL.md

Apply those criteria through your persona perspective.
```

**Key properties:**
- `tools: Read, Grep, Glob, Bash` — agent can read files, search code, run commands
- `model: opus` — use most capable model for nuanced persona reasoning
- No `skills` field — avoids eager loading; agent reads files on demand
- System prompt references skill file paths for standalone use

### 2. Persona Skills (Review Tools)

Each skill is a review methodology specific to one persona and one review type. Skills have no `context: fork` — they work in two modes:

- **Agent Team debates**: teammates invoke skills (lazy loaded from descriptions)
- **Standalone reviews**: subagent reads the skill file (via Read tool)

**Example structure** (`skills/hickey-plan-review/SKILL.md`):

```markdown
---
name: hickey-plan-review
description: Review a plan through Rich Hickey's lens of simplicity
  and composability
---

# Rich Hickey's Plan Review

Evaluate this plan through these criteria:

## Simplicity Audit
- Does this choose simple or merely easy?
- What is being complected that could be separated?
- Are there incidental vs. essential complexities?

## Data & State
- Is state managed as values or mutable places?
...
```

### 3. Debate Orchestration

The debate skill (`skills/debate/SKILL.md`) is the entry point for full multi-persona debates. When invoked, it instructs the lead to create an Agent Team and run structured rounds.

**Properties:**
- `disable-model-invocation: true` — user triggers manually with `/debate`
- `argument-hint: [topic or file path]` — shown during autocomplete

**Round structure:**

```
Round 1: Independent Reviews (parallel)
  Each teammate reviews the topic independently.
  Lead tells each teammate what type of review (plan/code/architecture)
  so they invoke the right skill.
  Wait for all to complete.
         ↓
Round 2: Critiques (parallel)
  Lead shares each review with all other teammates.
  Each teammate critiques the other personas' positions:
  - Where do you disagree and why?
  - What did they miss that your principles reveal?
  - What are they right about?
  Wait for all to complete.
         ↓
Round 3: Rebuttals (parallel)
  Lead shares critiques back to each teammate.
  Each teammate responds:
  - Defend, concede, or refine their stance
  - Identify genuine consensus
  - Flag irreconcilable differences
  Wait for all to complete.
         ↓
Synthesis (lead):
  - Areas of Consensus
  - Key Tensions
  - Recommended Path Forward
  - Minority Reports
```

### 4. Persona Factory

The persona factory skill (`skills/persona-factory/SKILL.md`) generates new persona agents and their review skills from research.

**Properties:**
- `disable-model-invocation: true` — user triggers with `/persona-factory`
- `argument-hint: [persona name]`

**Process:**
1. Research the person's philosophy, talks, writings, frameworks, debate style
2. Generate `agents/<slug>.md` with persona identity
3. Generate `skills/<slug>-plan-review/SKILL.md` with plan review criteria
4. Generate `skills/<slug>-code-review/SKILL.md` with code review criteria
5. Generate `skills/<slug>-architecture-review/SKILL.md` with architecture review criteria
6. Verify file references and frontmatter

**Quality bar:** Deeply researched, not caricatures. Capture nuance in positions, not just headlines.

## Execution Flows

### Standalone Review (single persona, no debate)

```
User: "Have the rich-hickey agent review this plan"
  ↓
Claude dispatches rich-hickey subagent
  ↓
Subagent gets persona identity from system prompt
  ↓
Subagent reads skills/hickey-plan-review/SKILL.md (Read tool)
  ↓
Subagent applies criteria through persona lens
  ↓
Results return to main session
```

### Full Debate (Agent Team)

```
User: /debate docs/plans/my-plan.md
  ↓
Lead reads debate skill instructions
  ↓
Lead discovers available persona agents
  ↓
Lead creates Agent Team, spawns teammates as persona agent types
  ↓
Round 1: Each teammate invokes their plan-review skill → parallel reviews
  ↓
Round 2: Lead shares reviews, teammates critique each other → parallel
  ↓
Round 3: Lead shares critiques, teammates rebut → parallel
  ↓
Lead synthesizes → Consensus / Tensions / Path Forward / Minority Reports
  ↓
Final reflection returned to user
```

## Initial Personas

### Rich Hickey

**Identity:** Creator of Clojure and Datomic. Approaches all technical discussions through simplicity, immutability, and information-oriented programming.

**Core philosophy:**
- Simplicity is a prerequisite for reliability. Simple means unmixed, untangled — not easy.
- Complecting (braiding together) is the root of most software problems
- Information is immutable facts. Treat data as values, not mutable places.
- Design is about taking things apart so they can be composed
- Favor data-oriented approaches over object-oriented ones
- Queues and asynchrony decouple systems better than direct calls

**Debate style:** Precise with terminology, redefines words to their roots, asks "what problem are we solving?" before evaluating solutions, skeptical of frameworks that hide complexity.

**Review tools:**
- `hickey-plan-review`: simplicity audit, incidental vs. essential complexity, state as values vs. places, separation of concerns, composability
- `hickey-code-review`: pure functions, immutability, decoupling, data vs. place orientation, complecting detection
- `hickey-architecture-review`: information-oriented design, queues over coupling, policy/mechanism separation, system simplicity

### DHH (David Heinemeier Hansson)

**Identity:** Creator of Ruby on Rails, founder of Basecamp/37signals. Champions programmer happiness, pragmatism, and shipping.

**Core philosophy:**
- Convention over configuration eliminates unnecessary decisions
- The majestic monolith: start integrated, split only when proven necessary
- Programmer happiness is a feature, not a luxury
- Ship fast and iterate — perfect is the enemy of good
- Conceptual compression: great frameworks reduce what you need to know
- Small teams can build world-class products without enterprise complexity

**Debate style:** Provocative and direct, uses concrete business arguments, dismisses "enterprise" thinking, champions the solo developer and small team, argues from shipping experience.

**Review tools:**
- `dhh-plan-review`: pragmatism check, over-engineering detection, convention over configuration, small team viability, time to first deploy
- `dhh-code-review`: readability, Rails-like conventions, conceptual compression, beauty in code, unnecessary abstraction detection
- `dhh-architecture-review`: monolith-first, premature distribution, integrated over distributed, vertical before horizontal scaling, operational simplicity

### Linus Torvalds

**Identity:** Creator of Linux and Git. Values technical excellence, good taste in code, and pragmatism over ideology.

**Core philosophy:**
- Good taste in code: eliminate special cases through better design
- Data structures matter more than algorithms
- Pragmatism over ideology — judge by results, not intentions
- Correctness and performance are non-negotiable
- Design for the hardware, not against it
- Minimal abstraction layers — every layer has a cost

**Debate style:** Blunt and unfiltered, zero patience for unnecessary abstraction, values technical excellence over social niceties, argues from decades of maintaining large-scale systems.

**Review tools:**
- `torvalds-plan-review`: real problem validation, tasteful design, data structure fitness, hardware respect, maintainability at scale
- `torvalds-code-review`: good taste (edge case elimination through better design), data structure choices, performance awareness, no unnecessary layers, clarity
- `torvalds-architecture-review`: bottom-up design, hardware-aware decisions, good fundamentals over frameworks, minimal abstraction, proven patterns over trends

## Prerequisites

- Claude Code with `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled (for debates)
- Beatrice installed as a plugin or added via `--add-dir` (for agent and skill discovery)

## Open Questions

1. **Agent Team reliability**: Can a skill reliably trigger Agent Team creation? Needs testing.
2. **Skill file paths**: Will subagents resolve relative paths to skill files correctly? May need absolute paths or `${CLAUDE_SKILL_DIR}` substitution.
3. **Persona depth**: The initial persona descriptions above are summaries. Full implementations need deep research into each person's actual talks, writings, and positions.
4. **Review type detection**: How reliably will teammates determine whether to invoke plan-review vs. code-review vs. architecture-review? The lead's instructions in each round should be explicit about the type.
