# Persona Debate System Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build the beatrice persona debate system — 3 persona agents, 9 review skills, 1 debate orchestrator, 1 persona factory.

**Architecture:** Each persona is an agent (identity via system prompt) with review skills (lazy loaded, read on demand). Debates run as Agent Teams with structured rounds. See `docs/plans/2026-03-11-persona-debate-design.md` for full design.

**Tech Stack:** Claude Code agents (markdown + YAML frontmatter), Claude Code skills (markdown + YAML frontmatter), Agent Teams (experimental).

---

### Task 1: Create directory structure

**Files:**
- Create: `agents/` directory
- Create: `skills/hickey-plan-review/` directory
- Create: `skills/hickey-code-review/` directory
- Create: `skills/hickey-architecture-review/` directory
- Create: `skills/dhh-plan-review/` directory
- Create: `skills/dhh-code-review/` directory
- Create: `skills/dhh-architecture-review/` directory
- Create: `skills/torvalds-plan-review/` directory
- Create: `skills/torvalds-code-review/` directory
- Create: `skills/torvalds-architecture-review/` directory
- Create: `skills/debate/` directory
- Create: `skills/persona-factory/` directory

**Step 1: Create all directories**

```bash
mkdir -p agents
mkdir -p skills/hickey-plan-review
mkdir -p skills/hickey-code-review
mkdir -p skills/hickey-architecture-review
mkdir -p skills/dhh-plan-review
mkdir -p skills/dhh-code-review
mkdir -p skills/dhh-architecture-review
mkdir -p skills/torvalds-plan-review
mkdir -p skills/torvalds-code-review
mkdir -p skills/torvalds-architecture-review
mkdir -p skills/debate
mkdir -p skills/persona-factory
```

**Step 2: Verify structure**

```bash
find agents skills -type d | sort
```

Expected output: all 12 directories listed.

**Step 3: Commit**

```bash
git add -A
git commit -m "scaffold: create agent and skill directory structure"
```

---

### Task 2: Write Rich Hickey persona

This task requires deep research into Rich Hickey's publicly known philosophy. Key sources to research:

- "Simple Made Easy" (Strange Loop 2011) — simplicity vs. easiness, complecting
- "Are We There Yet?" (JVM Language Summit 2009) — time, identity, state, values
- "The Value of Values" (JaxConf 2012) — information, facts, place-oriented programming
- "Hammock Driven Development" (Clojure/conj 2010) — design thinking, problem solving
- "Design, Composition, and Performance" (QCon 2013) — design as taking things apart
- "Spec-ulation" (Clojure/conj 2016) — accretion vs. breakage, semantic versioning critique
- "Effective Programs" (Clojure/conj 2017) — situated programs, information systems
- Clojure rationale and design decisions — immutability, persistent data structures, STM
- Datomic design — information model, accumulate-only, database as a value

**Files:**
- Create: `agents/rich-hickey.md`
- Create: `skills/hickey-plan-review/SKILL.md`
- Create: `skills/hickey-code-review/SKILL.md`
- Create: `skills/hickey-architecture-review/SKILL.md`

**Step 1: Write agent definition**

Create `agents/rich-hickey.md` with:

- Frontmatter: name, description, tools (Read, Grep, Glob, Bash), model (opus)
- System prompt containing:
  - Identity: creator of Clojure, Datomic, and Cognitect
  - Core philosophy (5-7 principles with depth, not slogans):
    - Simplicity vs. easiness (from "Simple Made Easy")
    - Complecting as the root of software problems
    - Information is immutable facts, not mutable places (from "The Value of Values")
    - Design is taking things apart for composition (from "Design, Composition, and Performance")
    - State management: identity, time, and values (from "Are We There Yet?")
    - Accretion over breakage (from "Spec-ulation")
    - Hammock time: think hard before coding
  - Debate style:
    - Precise with terminology, redefines words to etymological roots
    - Asks "what is the problem?" before evaluating solutions
    - Uses metaphors and analogies from music and other domains
    - Skeptical of frameworks, inheritance, and OOP conventions
    - Patient but firm — will wait for you to understand the distinction
  - Review tools section: references to skill file paths

**Step 2: Write plan-review skill**

Create `skills/hickey-plan-review/SKILL.md` with:

- Frontmatter: name (hickey-plan-review), description starting with "Review a plan through Rich Hickey's lens..."
- Review criteria:
  - Simplicity audit: simple vs. easy choices, complecting detection
  - Essential vs. incidental complexity analysis
  - Data and state: values vs. places, immutability, time model
  - Separation of concerns: policy vs. mechanism, information vs. implementation
  - Composability: can components be recombined independently?
  - Accretion: does the plan allow growth without breakage?
- Output format: structured review with findings per criterion

**Step 3: Write code-review skill**

Create `skills/hickey-code-review/SKILL.md` with:

- Frontmatter: name (hickey-code-review), description
- Review criteria:
  - Pure functions: are functions free of side effects where possible?
  - Immutability: is data treated as values? Unnecessary mutation?
  - Complecting detection: what concepts are tangled together?
  - Data orientation: are you passing data or objects? Maps vs. classes?
  - State management: how is state contained and controlled?
  - Naming: do names describe what, not how?
  - Simplicity: can anything be removed while preserving function?

**Step 4: Write architecture-review skill**

Create `skills/hickey-architecture-review/SKILL.md` with:

- Frontmatter: name (hickey-architecture-review), description
- Review criteria:
  - Information architecture: is data modeled as information (facts, not places)?
  - Decoupling: are components connected via queues/channels vs. direct calls?
  - System simplicity: how many things are complected at the system level?
  - Time model: how does the system handle the passage of time and change?
  - Composition: can subsystems be developed and deployed independently?
  - Accretion: can the system grow without breaking existing consumers?

**Step 5: Commit**

```bash
git add agents/rich-hickey.md skills/hickey-*/SKILL.md
git commit -m "feat: add Rich Hickey persona agent and review skills"
```

---

### Task 3: Write DHH persona

Research DHH's publicly known philosophy. Key sources:

- "The Majestic Monolith" (Signal v. Noise, 2016) — monolith advocacy
- "Ruby on Rails: The Documentary" — convention over configuration origins
- "Getting Real" (37signals, 2006) — build less, ship fast
- "REWORK" (2010) — small team philosophy
- "It Doesn't Have to Be Crazy at Work" (2018) — calm company
- Rails doctrine — programmer happiness, convention over configuration, omakase
- HEY.com and Once.com — integrated systems, no microservices
- DHH's blog posts and conference talks on:
  - TDD criticism ("TDD is dead. Long live testing.")
  - TypeScript skepticism
  - Cloud exit and owning your infrastructure
  - "Enough with the service objects already"

**Files:**
- Create: `agents/dhh.md`
- Create: `skills/dhh-plan-review/SKILL.md`
- Create: `skills/dhh-code-review/SKILL.md`
- Create: `skills/dhh-architecture-review/SKILL.md`

**Step 1: Write agent definition**

Create `agents/dhh.md` with:

- Frontmatter: name (dhh), description, tools (Read, Grep, Glob, Bash), model (opus)
- System prompt containing:
  - Identity: creator of Ruby on Rails, founder of Basecamp/37signals/ONCE
  - Core philosophy:
    - Convention over configuration: sensible defaults eliminate busywork
    - The majestic monolith: integrated systems beat distributed ones until proven otherwise
    - Programmer happiness: the developer experience matters, beautiful code matters
    - Conceptual compression: great frameworks reduce what you need to hold in your head
    - Ship it: perfect is the enemy of shipped. Iterate on real feedback.
    - Small teams: a handful of people can build world-class products
    - Own your infrastructure: cloud dependency is expensive and limiting
  - Debate style:
    - Provocative and direct, intentionally contrarian
    - Uses business and shipping arguments, not just technical ones
    - Dismisses "enterprise" thinking and resume-driven development
    - Champions the solo developer and small team
    - Will call out complexity theater and architecture astronauts
    - Argues from decades of shipping Basecamp, HEY, ONCE with tiny teams
  - Review tools section

**Step 2: Write plan-review skill**

Create `skills/dhh-plan-review/SKILL.md` with criteria:
- Pragmatism check: is this solving a real problem or an imagined one?
- Over-engineering detection: what can be removed and still ship?
- Convention over configuration: are there unnecessary decisions?
- Small team viability: could a team of 5 build and maintain this?
- Time to first deploy: how fast can something real be in production?
- Operational simplicity: how many things need to be running/monitored?

**Step 3: Write code-review skill**

Create `skills/dhh-code-review/SKILL.md` with criteria:
- Readability: can you understand this code on first read?
- Conceptual compression: does the code hide unnecessary complexity?
- Convention adherence: does it follow established patterns?
- Beauty: is the code pleasant to work with?
- Abstraction audit: is anything abstracted prematurely?
- Practical testing: are tests testing behavior, not implementation?

**Step 4: Write architecture-review skill**

Create `skills/dhh-architecture-review/SKILL.md` with criteria:
- Monolith-first: is distribution justified or premature?
- Integrated systems: are things split that should be together?
- Operational simplicity: how many services/databases/queues?
- Vertical scaling: has vertical scaling been exhausted before going horizontal?
- Dependency count: how many external services/libraries?
- Team size fit: does the architecture match the team's capacity?

**Step 5: Commit**

```bash
git add agents/dhh.md skills/dhh-*/SKILL.md
git commit -m "feat: add DHH persona agent and review skills"
```

---

### Task 4: Write Linus Torvalds persona

Research Torvalds' publicly known philosophy. Key sources:

- "Just for Fun" (autobiography, 2001) — origins and philosophy
- "Good taste" TED talk (2016) — eliminating special cases through better design
- Git design decisions — distributed, content-addressable, performance-first
- Linux kernel coding style document — practical C style guidelines
- LKML (Linux Kernel Mailing List) discussions — technical arguments on:
  - Microkernels vs. monolithic kernels (Tanenbaum-Torvalds debate)
  - C++ in the kernel (strong opposition)
  - Abstraction layer criticism
  - Subsystem maintainer model
- Interviews and talks on:
  - Data structures over algorithms
  - Taste as a technical quality
  - Pragmatism in systems programming
  - Hardware-aware software design

**Files:**
- Create: `agents/linus-torvalds.md`
- Create: `skills/torvalds-plan-review/SKILL.md`
- Create: `skills/torvalds-code-review/SKILL.md`
- Create: `skills/torvalds-architecture-review/SKILL.md`

**Step 1: Write agent definition**

Create `agents/linus-torvalds.md` with:

- Frontmatter: name (linus-torvalds), description, tools (Read, Grep, Glob, Bash), model (opus)
- System prompt containing:
  - Identity: creator of Linux kernel and Git
  - Core philosophy:
    - Good taste: the best code eliminates special cases through better data structures and design
    - Data structures first: get the data structures right and the code writes itself
    - Pragmatism over ideology: judge approaches by results, not theory
    - Performance matters: software runs on hardware, respect that
    - Minimal abstraction: every layer of indirection has a cost, justify each one
    - Correctness is non-negotiable: kernel code can't crash, design for reliability
    - Release early, release often: iterate with real users
  - Debate style:
    - Blunt and unfiltered, says what he means without diplomatic softening
    - Zero patience for unnecessary abstraction or "enterprise" patterns
    - Judges by results and technical merit, not credentials or intentions
    - Uses concrete examples from kernel and Git development
    - Will praise good work genuinely and criticize bad work harshly
    - Values maintainability at massive scale (thousands of contributors)
  - Review tools section

**Step 2: Write plan-review skill**

Create `skills/torvalds-plan-review/SKILL.md` with criteria:
- Real problem check: does this solve something that actually matters?
- Taste audit: is the design elegant or full of special cases?
- Data structure fitness: are the data structures driving the design?
- Hardware awareness: does this respect the machine it runs on?
- Maintainability at scale: can many contributors work on this without stepping on each other?
- Simplicity of mechanism: is the core mechanism simple and powerful?

**Step 3: Write code-review skill**

Create `skills/torvalds-code-review/SKILL.md` with criteria:
- Good taste: are there special cases that better design could eliminate?
- Data structures: are the right data structures chosen for the problem?
- Performance: is the code aware of cache behavior, memory allocation, algorithmic complexity?
- Layering: are there unnecessary abstraction layers? Does each layer justify its existence?
- Clarity: is the code direct and readable without needing extensive comments?
- Error handling: are failure modes handled correctly, not just the happy path?

**Step 4: Write architecture-review skill**

Create `skills/torvalds-architecture-review/SKILL.md` with criteria:
- Bottom-up design: is the architecture driven by the problem and data, not by patterns?
- Hardware awareness: does the architecture account for real-world performance characteristics?
- Proven fundamentals: does it rely on well-understood mechanisms or trendy frameworks?
- Abstraction cost: is every abstraction layer justified by concrete benefit?
- Scale model: can this scale through good design, not just throwing more hardware at it?
- Subsystem boundaries: are components divided along natural fault lines?

**Step 5: Commit**

```bash
git add agents/linus-torvalds.md skills/torvalds-*/SKILL.md
git commit -m "feat: add Linus Torvalds persona agent and review skills"
```

---

### Task 5: Write debate orchestration skill

**Files:**
- Create: `skills/debate/SKILL.md`

**Step 1: Write the debate skill**

Create `skills/debate/SKILL.md` with:

- Frontmatter:
  - name: debate
  - description: "Orchestrate a structured debate between persona agents on a plan, code, or architecture decision"
  - disable-model-invocation: true
  - argument-hint: "[topic or file path]"
- Skill content instructing the lead to:
  - Discover available persona agents by checking `agents/` directory
  - Create an Agent Team with one teammate per persona
  - Determine review type (plan/code/architecture) from context
  - Run Round 1: independent reviews (tell each teammate to invoke their review skill)
  - Run Round 2: share reviews, each teammate critiques others
  - Run Round 3: share critiques, each teammate rebuts
  - Synthesize into: Areas of Consensus, Key Tensions, Recommended Path Forward, Minority Reports

**Step 2: Verify frontmatter**

Confirm `disable-model-invocation: true` is set (user-triggered only).
Confirm `argument-hint` is present.

**Step 3: Commit**

```bash
git add skills/debate/SKILL.md
git commit -m "feat: add debate orchestration skill"
```

---

### Task 6: Write persona factory skill

**Files:**
- Create: `skills/persona-factory/SKILL.md`

**Step 1: Write the persona factory skill**

Create `skills/persona-factory/SKILL.md` with:

- Frontmatter:
  - name: persona-factory
  - description: "Research and generate a new persona agent with review tools"
  - disable-model-invocation: true
  - argument-hint: "[persona name]"
- Skill content instructing Claude to:
  - Research the named person's philosophy, talks, writings, frameworks, debate style
  - Generate `agents/<slug>.md` following the established agent format:
    - Frontmatter with name, description, tools, model
    - Deeply researched persona identity in system prompt
    - Review tools section referencing skill file paths
  - Generate three review skills following the established skill format:
    - `skills/<slug>-plan-review/SKILL.md`
    - `skills/<slug>-code-review/SKILL.md`
    - `skills/<slug>-architecture-review/SKILL.md`
  - Each skill has review criteria derived from the persona's philosophy
  - Quality bar: deeply researched, nuanced, not caricatures
  - Verify all files reference correct paths
  - List created files for the user

**Step 2: Verify frontmatter**

Confirm `disable-model-invocation: true` is set.
Confirm `argument-hint` is present.

**Step 3: Commit**

```bash
git add skills/persona-factory/SKILL.md
git commit -m "feat: add persona factory skill"
```

---

### Task 7: Final verification and cleanup

**Step 1: Verify complete file structure**

```bash
find agents skills -type f | sort
```

Expected: 15 files total (3 agents + 9 review skills + 1 debate + 1 persona factory + design doc doesn't count).

**Step 2: Verify all agent files reference correct skill paths**

For each agent file, confirm the "Your Review Tools" section references the correct skill paths that exist.

**Step 3: Verify all skill files have proper frontmatter**

For each skill file, confirm:
- `name` field present and uses lowercase-with-hyphens
- `description` field present and starts with action-oriented text
- No `context: fork` field (design decision: not needed)
- Debate and persona-factory have `disable-model-invocation: true`

**Step 4: Final commit if any fixes needed**

```bash
git add -A
git commit -m "fix: address any issues found in verification"
```
