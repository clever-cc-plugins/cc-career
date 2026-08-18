---
name: career-plan
description: >
  Use this skill to build or refine a structured career plan or uplevel roadmap —
  direction, prioritized skills to develop, and a milestone timeline. Invoke when the
  user asks to "build a career plan", "plan my uplevel", "what should I develop next",
  or "create a roadmap for my career". Produces a career plan and registers it as
  project context so cc-coach's career-coach and cc-content/cc-concept skills can use
  it.
allowed-tools: Read, Write, Edit, Bash
argument-hint: "[optional: focus area or target role for the plan]"
---

# Career Plan Skill

This skill produces a career plan — direction, prioritized skills to develop, and a
milestone timeline — grounded in the person's career background and goals. It
registers the output as project context for reuse by `cc-coach:career-coach` and by
`cc-content`/`cc-concept` skills when personal-branding-strategy.md references it.

This skill follows the shared step sequence in `../_shared/skill-contract.md`. Steps
below add only career-plan-specific behavior.

## Step 0-1: Recall learnings and load context

If `.claude/learnings.md` exists, read it silently. Apply relevant entries — in
particular, framework preferences, rejected directions, or constraints discovered in
prior runs. Do not announce this step. If the file is absent, continue normally.

Read the `## Context files` table in `CLAUDE.md`. Match the following needs against
the **Summary** column semantically — never against a label or filename:

- **Career background** — current role, industry, experience, responsibilities, what
  has felt satisfying vs. unsatisfying
- **Career goals** — target direction, timeframe, what's driving the goal

Note silently if present (do not ask about): a prior `career-performance-review`
output, if registered — factor validated/invalidated bets from the last plan into
this one.

## Step 2: Gating — Three-State Model

This skill gates on **exactly two** needs: **career background** and **career
goals**. For each, resolve one of three states:

1. **Covered** — present in the context table (Summary match) → use silently, no ask.
2. **Absent-but-supplied** — ask **once** (never twice in one invocation); user
   answers → use, non-degraded.
3. **Absent-and-unanswered** — asked once, no usable answer → that section is
   labelled **⚠ DEGRADED**, never fabricated, and the rest of the plan still
   generates.

### Check career background

Search the context table Summary column for semantic matches: career background,
current role, experience, responsibilities, work history, job title.

If **covered**, use silently.

If **absent**, offer once to delegate:

> "I don't see your career background loaded yet. Want me to run
> `/career-onboarding` first? It's a short interview and the answer feeds every plan
> or review you run after. This is optional — I can work with whatever you tell me
> directly instead."

- **Accepted** → the user runs (or consents to) `/career-onboarding`. Pause here and
  wait for the background to be registered; then re-run this skill.
- **Declined** → ask once (only if declined):

  > "Understood. Briefly: What's your current role and industry? Roughly how many
  > years of experience? What are your core responsibilities? What's felt most and
  > least satisfying in your work so far?"

  If the user provides a usable answer, proceed **non-degraded**. If they decline or
  the answer is too vague to use, label the section `## Career Background ⚠ DEGRADED`
  in the output and continue.

### Check career goals

Search the context table Summary column for semantic matches: career goals,
aspirations, target role, direction, timeframe.

If **covered**, use silently.

If **absent**, ask once:

> "What direction are you aiming for? Where do you want to be in 1-2 years, and in
> 5? What's driving that? Any deadline or timeframe attached?"

If the user provides a usable answer, proceed **non-degraded**. If they decline or
the answer is too vague to use, label the section `## Career Goals ⚠ DEGRADED` in the
output and continue.

## Step 3: Calibrate

Consult the focus area or target role in `$ARGUMENTS`, if present. Set plan depth
and scope:

- **Narrow focus** (e.g. "just the next 6 months" or "focus on technical skills") →
  scope the plan tightly to that window or dimension.
- **Broad or no focus** → develop a full plan covering direction, near-term skills,
  and milestones across the timeframe implied by the loaded goals.

## Step 4: Generate

Apply Career Anchors and the Skill-Will Matrix from
`../_shared/career-planning-frameworks.md`. **State each choice before generating.**
Example:

> "Using Career Anchors to set direction; Skill-Will Matrix to sequence near-term
> development."

Produce a career plan that includes:

- **Career Anchor** — the inferred dominant anchor and one sentence of supporting
  evidence, with a note if the stated goal pulls against it.
- **Direction statement** — the target role or trajectory this plan aims at.
- **Prioritized skills** — 3-6 candidate skills plotted and sequenced per the
  Skill-Will Matrix (near-term first, delegate-or-drop items noted separately).
- **Milestones** — at least three, each tied to a rough timeframe consistent with the
  loaded goals.

## Step 5-6: Quality check and delimited output

Review the generated plan for:

- **Completeness**: All four elements above are present (Career Anchor, direction
  statement, prioritized skills, milestones).
- **Milestone count**: At least three milestones are listed.
- **Consistency**: The prioritized skills and milestones both serve the stated
  direction; nothing pulls against the identified Career Anchor without a flag.

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

Return the career plan inside clear delimiters:

```
─────────────────────────────────────────────────────────────────
## Career Plan: [Direction / Target Role]
─────────────────────────────────────────────────────────────────

[Generated career plan here]

─────────────────────────────────────────────────────────────────
```

## Step 6.5: Register output

Write the career plan to `context/career-plan.md` by convention. **Apply
collision-check logic**:

1. Check if `context/career-plan.md` exists.
2. If it exists, generate a distinct filename by appending a `-N` suffix before
   `.md` (try `-2`, then `-3`, etc.) until a filename that does NOT already exist is
   found. Never overwrite an existing plan.
3. Write to the final, collision-checked filename.

Confirm the file was created, then add a row to the `## Context files` table in
`CLAUDE.md`. Use this format:

```markdown
| Career plan | `context/career-plan.md` | Career direction, Career Anchor, prioritized skills, and milestone timeline |
```

The Summary must be semantic and describe what the file covers — use keywords so
`cc-coach:career-coach`'s coverage check will find it. Do **not** hand-edit the
**Key Config Files** table — the pre-commit hook owns that sync.

## Step 7: Feedback

Store learnings tagged `[cc-career:career-plan]` in `.claude/learnings.md`. Examples:

```
[cc-career:career-plan] user rejected the management-track anchor inference, confirmed Technical/Functional instead — 2026-08-18
[cc-career:career-plan] user wants milestones expressed as quarter offsets (Q1, Q2) rather than calendar dates — 2026-08-18
```

If the session revealed a rejected inference, framework preference, or constraint
that future runs should apply, record it now.
