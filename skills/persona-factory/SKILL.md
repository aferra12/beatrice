---
name: persona-factory
description: Research and generate a new persona agent with review tools
argument-hint: [persona name, e.g. "Linus Torvalds"]
---

# Persona Factory

Generate a complete persona agent and review skill set for the named person.

## Step 0: Study Existing Examples

Before generating anything, read these files to understand the expected format, depth, and tone:

- Read `agents/rich-hickey.md` as the reference agent definition
- Read `skills/hickey-plan-review/SKILL.md` as the reference plan review skill
- Read `skills/hickey-code-review/SKILL.md` as the reference code review skill
- Read `skills/hickey-architecture-review/SKILL.md` as the reference architecture review skill

Also read `agents/dhh.md` and `agents/linus-torvalds.md` for additional examples of how different personas translate into different agent voices, philosophies, and review criteria.

You must match the depth and specificity of these examples. Generic, surface-level personas are not acceptable.

## Step 1: Research the Person

Research the named person's publicly known philosophy, principles, and positions. Focus on:

- **Core beliefs and values**: What do they care most deeply about? What hills do they die on?
- **Notable talks, writings, and frameworks**: Specific titles, key arguments, memorable phrases
- **Signature arguments and recurring themes**: The ideas they return to again and again across years of work
- **Known stances on common debates**: Where do they land on the contested questions in their field?
- **Communication and debate style**: How do they argue? Are they blunt, Socratic, patient, provocative? What rhetorical tools do they use?
- **Nuance and subtlety**: Capture what they actually believe, not the caricature. "Hickey doesn't hate objects, he hates complecting state with identity." "Torvalds doesn't hate abstraction, he hates abstraction that doesn't pay for itself." That level of precision.

The research must be deep. Shallow bullet points produce shallow personas. Every principle you include should reference a specific talk, paper, book, or well-known position.

## Step 2: Determine the Slug

Convert the person's name to a lowercase, hyphenated slug. Use the name they are most commonly known by in the software community:

- "Rich Hickey" -> `rich-hickey`
- "David Heinemeier Hansson" -> `dhh`
- "Linus Torvalds" -> `linus-torvalds`
- "Martin Fowler" -> `martin-fowler`
- "Leslie Lamport" -> `leslie-lamport`

Use your judgment. The slug should be what developers would naturally type.

## Step 3: Generate Agent Definition

Create `agents/<slug>.md` with the following structure:

### Frontmatter

```yaml
---
name: <slug>
description: <Full Name> persona for reviewing plans, code, and architecture through <their key lens -- 2-4 words capturing their core focus>.
tools: Read, Grep, Glob, Bash
model: opus
---
```

### System Prompt Body

The body must contain these sections in order:

**Opening paragraph (1-2 sentences):** Establish identity. Who they are, what they created or are known for, and the core disposition that shapes their worldview. Write in second person ("You are..."). See how the existing agents do this -- it is not a bio, it is a voice activation.

**Core Philosophy (5-7 principles):** Each principle gets a heading and a substantial paragraph (4-8 sentences). Each principle must:
- State the principle clearly
- Explain the reasoning behind it, not just the conclusion
- Reference specific talks, writings, frameworks, or well-known positions where possible
- Include the nuance -- what people commonly get wrong about this person's position
- Connect to concrete software decisions, not just abstract philosophy

Do NOT write generic principles like "write clean code" or "keep it simple." Write principles that could only belong to this specific person. If you could swap the name and the principle would still work, it is too generic.

**Debate Style (1-2 paragraphs):** How do they argue? What rhetorical tools do they use? What triggers them? What do they respect in an opponent? What do they dismiss? Are they blunt, diplomatic, Socratic, provocative, patient? Do they use analogies, etymology, concrete examples, academic references, business arguments? Capture their actual voice, not a sanitized version of it.

**Your Review Tools section:** Exactly this format:

```markdown
## Your Review Tools

When asked to review something, determine the type and read the relevant methodology file:
- Plan review: read skills/<slug>-plan-review/SKILL.md
- Code review: read skills/<slug>-code-review/SKILL.md
- Architecture review: read skills/<slug>-architecture-review/SKILL.md

Apply those criteria through your persona perspective.
```

## Step 4: Generate Plan Review Skill

Create directory `skills/<slug>-plan-review/` and file `SKILL.md` inside it.

### Frontmatter

```yaml
---
name: <slug>-plan-review
description: Review a plan through <Full Name>'s lens of <key principles -- short phrase>
---
```

### Body

Start with a heading and a short instruction paragraph telling Claude to evaluate the plan through the following criteria, citing specific parts of the plan.

Then provide **5-8 review criteria**, each as a `##` heading with a substantial paragraph (3-6 sentences). Each criterion must:
- Be derived from this person's specific philosophy, not generic review advice
- Explain what to look for and why it matters from this person's perspective
- Be actionable -- tell the reviewer what to flag and what to praise

End with an **Output Format** section specifying a structured output template with sections matching the criteria, plus a Summary section.

## Step 5: Generate Code Review Skill

Create directory `skills/<slug>-code-review/` and file `SKILL.md` inside it.

### Frontmatter

```yaml
---
name: <slug>-code-review
description: Review code through <Full Name>'s lens of <key principles -- short phrase>
---
```

### Body

Same structure as the plan review: heading, instruction paragraph, 5-8 review criteria as `##` headings with paragraphs, and an Output Format section. The criteria must be specific to code review -- what would this person look for when reading actual code? What patterns would they flag? What would they praise?

## Step 6: Generate Architecture Review Skill

Create directory `skills/<slug>-architecture-review/` and file `SKILL.md` inside it.

### Frontmatter

```yaml
---
name: <slug>-architecture-review
description: Review architecture through <Full Name>'s lens of <key principles -- short phrase>
---
```

### Body

Same structure: heading, instruction paragraph, 5-8 review criteria, Output Format section. The criteria must be specific to architecture review -- system boundaries, component interactions, deployment models, scaling strategies, data flow, operational concerns -- all viewed through this person's lens.

## Step 7: Verification

After generating all files, verify:

1. The agent file at `agents/<slug>.md` exists and its "Your Review Tools" section references the correct three skill paths
2. `skills/<slug>-plan-review/SKILL.md` exists with proper frontmatter (name matches `<slug>-plan-review`)
3. `skills/<slug>-code-review/SKILL.md` exists with proper frontmatter (name matches `<slug>-code-review`)
4. `skills/<slug>-architecture-review/SKILL.md` exists with proper frontmatter (name matches `<slug>-architecture-review`)
5. All four files have content -- no placeholders, no TODOs, no "fill in later"

List all created files for the user.

## Step 8: Commit

Stage and commit all new files:

```bash
git add agents/<slug>.md skills/<slug>-plan-review/SKILL.md skills/<slug>-code-review/SKILL.md skills/<slug>-architecture-review/SKILL.md
git commit -m "feat: add <Full Name> persona agent and review skills"
```

## Quality Bar

The quality bar is set by the existing personas. Read them. Match them. A good persona:

- Sounds like the actual person, not a Wikipedia summary of them
- Has principles that are specific enough to generate different reviews than any other persona would
- Captures what they actually believe, including the nuance that distinguishes their real position from the popular caricature
- Produces review criteria that lead to actionable, concrete feedback -- not generic advice
- Could not have its name swapped with another persona without the content becoming obviously wrong
