---
name: personal-branding-strategy
description: >
  Use this skill to define how the person wants to be perceived professionally — a
  personal brand Why/How/What and key themes their content and talks should
  reinforce. Invoke when the user asks to "define my personal brand", "work on my
  personal branding", "what should I be known for", or "give me themes for my
  content". Produces a personal-branding-strategy document and registers it as
  project context so cc-content/cc-concept skills can align individual pieces to it.
allowed-tools: Read, Write, Edit, Bash
argument-hint: "[optional: brief or focus for the branding work]"
---

# Personal Branding Strategy Skill

This skill produces a personal-branding-strategy document — the frame individual
content pieces execute against. It generates a Why/How/What brand message grounded
in the person's career background and goals, and registers the output as project
context for reuse by `cc-content` and `cc-concept` skills, and by
`cc-coach:career-coach`.

This skill follows the shared step sequence in `../_shared/skill-contract.md`. Steps
below add only branding-specific behavior.

## Step 0-1: Recall learnings and load context

If `.claude/learnings.md` exists, read it silently. Apply relevant entries — in
particular, branding preferences, rejected framings, or discovered constraints. Do
not announce this step. If the file is absent, continue normally.

Read the `## Context files` table in `CLAUDE.md`. Match the following needs against
the **Summary** column semantically — never against a label or filename:

- **Career background** — current role, industry, experience, responsibilities, what
  has felt satisfying vs. unsatisfying
- **Career goals** — target direction, timeframe, what's driving the goal

Note silently if present (do not ask about): a registered `career-plan` output — if
present, use its Career Anchor finding as an input to Step 4 rather than re-deriving
it.

## Step 2: Gating — Three-State Model

This skill gates on **exactly two** needs: **career background** and **career
goals**. For each, resolve one of three states:

1. **Covered** — present in the context table (Summary match) → use silently, no ask.
2. **Absent-but-supplied** — ask **once** (never twice in one invocation); user
   answers → use, non-degraded.
3. **Absent-and-unanswered** — asked once, no usable answer → that section is
   labelled **⚠ DEGRADED**, never fabricated, and the rest of the document still
   generates.

### Check career background

Search the context table Summary column for semantic matches: career background,
current role, experience, responsibilities, work history, job title.

If **covered**, use silently.

If **absent**, offer once to delegate:

> "I don't see your career background loaded yet. Want me to run
> `/career-onboarding` first? This is optional — I can work with whatever you tell me
> directly instead."

- **Accepted** → the user runs (or consents to) `/career-onboarding`. Pause here and
  wait for the background to be registered; then re-run this skill.
- **Declined** → ask once (only if declined):

  > "Understood. Briefly: What's your current role and industry? What are your core
  > responsibilities? What's felt most satisfying in your work so far?"

  If the user provides a usable answer, proceed **non-degraded**. If they decline or
  the answer is too vague to use, label the section `## Career Background ⚠ DEGRADED`
  in the output and continue.

### Check career goals

Search the context table Summary column for semantic matches: career goals,
aspirations, target role, direction, timeframe.

If **covered**, use silently.

If **absent**, ask once:

> "What's this brand supposed to support — a job search, speaking opportunities,
> building authority in a niche, something else? Where are you aiming to be in the
> next 1-2 years?"

If the user provides a usable answer, proceed **non-degraded**. If they decline or
the answer is too vague to use, label the section `## Career Goals ⚠ DEGRADED` in the
output and continue.

## Step 3: Calibrate

Consult the brief in `$ARGUMENTS`, if present. Set branding depth and scope:

- **Narrow brief** (e.g. "just refine my LinkedIn headline themes") → scope tightly
  to that surface.
- **No brief** → develop a full brand message and theme set, since this document
  becomes the frame other content skills reference going forward.

## Step 4: Generate

Select a framework from `../_shared/personal-branding-frameworks.md` following that
file's selection process — usually both apply together. **State each choice before
generating.** Example:

> "Using the Golden Circle to draft the brand message; Personal SWOT to stress-test
> it."

If a `career-plan` output is loaded (checked at Step 1), use its Career Anchor
finding directly as the Why's foundation rather than re-deriving it from scratch.

Generate a personal-branding-strategy document that includes:

- **Why** — the belief or purpose driving the work (not a job title).
- **How** — the distinctive approach or lens.
- **What** — current and near-term visible output (roles, projects, content types),
  each checked against the Why.
- **Personal SWOT** — Strengths, Weaknesses, Opportunities, Threats per the
  framework's how-to-apply steps.
- **Key themes** — 2-4 content themes derived from where a Strength meets an
  Opportunity, phrased so `cc-content` skills can use them directly (e.g. "industry
  war stories with a technical lens," not "be more visible").

## Step 5-6: Quality check and delimited output

Review the generated document for:

- **Completeness**: All five elements above are present (Why, How, What, Personal
  SWOT, key themes).
- **Traceability**: Every item under What traces back to the Why; no theme
  overclaims past a named Weakness.
- **Specificity**: Themes are phrased concretely enough for a content skill to use
  without further interpretation.

Fix any gaps before proceeding.

If Step 2 ended in a degraded state for one or both gated needs (career background
and/or career goals), prefix the output with:

```
⚠ DEGRADED OUTPUT — generated without: <comma-separated list of unmet needs>
```

For example, if career goals is degraded but career background is not, prefix with:
`⚠ DEGRADED OUTPUT — generated without: career goals`

If both are degraded, prefix with:
`⚠ DEGRADED OUTPUT — generated without: career background, career goals`

If neither need is degraded, omit the prefix.

Return the document inside clear delimiters:

```
─────────────────────────────────────────────────────────────────
## Personal Branding Strategy
─────────────────────────────────────────────────────────────────

[Generated personal-branding-strategy document here]

─────────────────────────────────────────────────────────────────
```

## Step 6.5: Register output

Write the document to `context/personal-branding-strategy.md` by convention. **Apply
collision-check logic**:

1. Check if `context/personal-branding-strategy.md` exists.
2. If it exists, generate a distinct filename by appending a `-N` suffix before
   `.md` (try `-2`, then `-3`, etc.) until a filename that does NOT already exist is
   found. Never overwrite an existing file.
3. Write to the final, collision-checked filename.

Confirm the file was created, then add a row to the `## Context files` table in
`CLAUDE.md`. Use this format:

```markdown
| Personal branding strategy | `context/personal-branding-strategy.md` | Personal brand Why/How/What, Personal SWOT, and content themes |
```

The Summary must explicitly name "personal brand" and "themes" — this wording is
what makes the file legible to `cc-content` skills and `cc-coach:career-coach`'s
coverage check; it is functionally part of this requirement, not cosmetic. Do
**not** hand-edit the **Key Config Files** table — the pre-commit hook owns that
sync.

## Step 7: Feedback

Store learnings tagged `[cc-career:personal-branding-strategy]` in
`.claude/learnings.md`. Examples:

```
[cc-career:personal-branding-strategy] user rejected "thought leader" framing, prefers "practitioner sharing what works" — 2026-08-18
[cc-career:personal-branding-strategy] user's Weakness (limited public speaking history) means themes should lean written content first — 2026-08-18
```

If the session revealed a rejected framing, theme preference, or constraint that
future runs should apply, record it now.
