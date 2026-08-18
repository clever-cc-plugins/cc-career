---
name: career-performance-review
description: >
  Use this skill to review whether a career plan's milestones actually played out.
  Invoke when the user says "review my career plan progress", "did I hit my
  milestones", "how's my uplevel going", or "career performance review". Works from
  whatever the owner reports about their own progress — there is no external data
  source. Do NOT use it to build or revise the plan itself — that stays
  `career-plan`'s job; this skill only verdicts the existing plan and recommends
  what to feed into the next run.
allowed-tools: Read, Write, Edit, Bash
argument-hint: "[optional: notes on progress since the last review]"
---

# Career Performance Review Skill

This skill closes the loop `career-plan`'s Step 7 (feedback) opens but never
resolves: whether the milestones a career plan committed to actually happened. It
never regenerates the career plan itself — that stays `career-plan`'s job. It names
which milestones held up and recommends the specific adjustment to feed into
`career-plan`'s next run.

This skill adapts `../_shared/skill-contract.md`'s 8-step sequence rather than
following it literally, in the same way `strategy-performance-review` does in
`cc-concept` — Steps 1-2 below extract milestones from a single plan document rather
than gating on background/goals needs.

## Step 0: Recall learnings

If `.claude/learnings.md` exists, read it silently. Apply relevant entries — in
particular, prior review scope choices. Do not announce this step. If the file is
absent, continue normally.

## Step 1: Load the plan

Read the `## Context files` table in `CLAUDE.md`. Find the registered `career-plan`
output by matching its Summary — never its label or filename.

If nothing is registered, say so and stop — there is no baseline to check progress
against:

> "I don't see a career plan registered yet. Run `/career-plan` first, then come
> back to review progress against it."

If more than one `career-plan` output is registered (e.g. from a prior collision-check
`-2` file), ask which one to review, showing both filenames and their first-line
direction statements.

## Step 2: Extract the milestones

Read the plan in scope and extract each milestone in this uniform shape:

```
Bet: <the specific milestone claim>
Expected signal: <what result would look like if the milestone was hit>
Source: career plan §Milestones
```

Present the full extracted list **before** moving to Step 3 — the owner can correct a
misread milestone before any progress gets compared against it.

## Step 3: Gather progress

Ask for whatever the owner has against the extracted milestones:

> "What actually happened against these? Paste whatever you have — or just tell me
> your read on how each one went."

If `$ARGUMENTS` already contains progress notes, use them as the starting point and
ask only about milestones not covered there. If notes only cover some of the
extracted milestones, proceed with what's available. Note which milestones remain
unevaluated — never fabricate a verdict for a milestone with no data.

## Step 4: Verdict per milestone

For each milestone with data, assign exactly one of: **Hit**, **Missed**,
**In progress** — with the evidence cited. For each milestone without data:
**Unevaluated**.

**Never average verdicts across milestones into one score.** A plan with two
milestones hit and one missed gets three separate verdicts, not a 67%.

For each Hit or In-progress milestone, note "no change needed" or a minor
refinement. For each Missed milestone, recommend re-running `career-plan` with the
specific adjustment named (e.g. "re-run `/career-plan` with the timeline extended by
one quarter" or "re-run `/career-plan` — the Skill-Will priority for X was wrong, will
was lower than assumed" — not a generic "reconsider this milestone").

## Step 5: Internal quality check

Confirm every verdict traces to either the extracted milestone (Step 2) or the
reported progress (Step 3) — no verdict should introduce a claim that wasn't stated
in either. Fix any gaps before proceeding.

## Step 6: Delimited output

```
─────────────────────────────────────────────────────────────────
Career performance review — <plan reviewed>
─────────────────────────────────────────────────────────────────

Bet: <milestone>
Verdict: Hit | Missed | In progress | Unevaluated
Evidence: <what the reported progress showed>
Recommendation: <re-run `/career-plan` with <specific adjustment> | no change needed>

[repeat per milestone]

─────────────────────────────────────────────────────────────────
```

## Register output

Write the review to `context/career-performance-review.md` by convention. Apply the
same collision-check logic as `career-plan`'s Step 6.5:

1. Check if `context/career-performance-review.md` exists.
2. If it exists, generate a distinct filename by appending a `-N` suffix before
   `.md` (try `-2`, then `-3`, etc.) until a filename that does NOT already exist is
   found. Never overwrite an existing review.
3. Write to the final, collision-checked filename.

Confirm the file was created, then add a row to the `## Context files` table in
`CLAUDE.md`:

```markdown
| Career performance review | `context/career-performance-review.md` | Verdicts on which career-plan milestones were hit, missed, or in progress |
```

The Summary must be semantic and describe what the file covers. Do **not**
hand-edit the **Key Config Files** table — the pre-commit hook owns that sync.

## Step 7: Feedback

Store learnings tagged `[cc-career:career-performance-review]` in
`.claude/learnings.md`. Examples:

```
[cc-career:career-performance-review] user's milestones consistently slip by about one quarter; account for that when re-planning — 2026-08-18
[cc-career:career-performance-review] user prefers reviewing quarterly, not per-milestone as they land — 2026-08-18
```

If the session revealed a review-scope preference or a recurring pattern in how
milestones slip, record it now.
