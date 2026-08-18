---
name: career-onboarding
description: >
  Use this skill to set up the career context this plugin's other skills need before
  planning, branding, or review work. Invoke when the user asks to "set up career
  context", "onboard for career planning", or when another cc-career skill reports a
  missing career background or goals need. Runs standalone and also recognises
  context an existing project already provides.
allowed-tools: Read, Write, Edit, Bash
argument-hint: "[optional: path to an existing file to register as career context]"
---

# Career Onboarding Skill

This skill collects the career context downstream cc-career skills need to run —
`career-plan`, `personal-branding-strategy`, and `career-performance-review`. Context
files are Markdown documents registered in the `## Context files` table of `CLAUDE.md`.
Skills load them on demand to plan accurately instead of defaulting to generic
questions on every run. This skill runs standalone and also recognizes context that
an existing project already provides.

This skill follows the shared step sequence in `../_shared/skill-contract.md`. Steps
below add only onboarding-specific behavior. (Steps 5 and 6 from the contract are
omitted because this skill collects multiple context files rather than emitting a
single artifact.)

## Step 0: Recall learnings

If `.claude/learnings.md` exists, read it silently. Apply relevant entries — in
particular, path preferences, rejected interview answers, or constraints discovered
in prior runs. Do not announce this step. If the file is absent, continue normally.

## Step 1: Discover existing context

This skill runs inside the owner's project — an arbitrary repo that may already have
its own structure, possibly with more than one `CLAUDE.md` at different levels. Never
assume a single root-level file. Scan the whole project:

```bash
find . -name 'CLAUDE.md' -not -path '*/.git/*' -not -path '*/node_modules/*' | sort
find . -type d -iname 'context' -not -path '*/.git/*' -not -path '*/node_modules/*' | sort
```

For each `context/`-like folder found, list its files too — the `## Context files`
table only shows registered entries, and a folder can hold files nobody registered
yet:

```bash
for d in <each context-like folder path from the find above>; do
  echo "=== $d ==="
  find "$d" -type f -name '*.md' | sort
done
```

For each `CLAUDE.md` found, check whether it already has a `## Context files` table:

```bash
for f in <each CLAUDE.md path from the find above>; do
  echo "=== $f ==="
  grep -A 200 '## Context files' "$f" 2>/dev/null || echo "(no context table)"
done
```

Keep the full list of discovered `CLAUDE.md` paths — Step 4 needs it to decide where
each newly registered file belongs. Show the owner the full picture, grouped by
location:

> "Here's what your project already has set up:
>
> **Root** (`CLAUDE.md`)
>
> | Label                        | File | Summary |
> | ---------------------------- | ---- | ------- |
> | <existing rows, or "(none)"> |
>
> (and nested locations if found)"

## Step 2: Coverage check

For each gated need below, decide **covered / uncovered** by semantic match against
the Summary column of the context table — never against a label or filename. The two
needs are evaluated **independently** — one being covered does not exempt the other.

**Gated need: Career background** — current role, industry, years of experience,
core responsibilities, and what has felt satisfying vs. unsatisfying in past work.
Keywords in Summary: career background, current role, experience, responsibilities,
work history, job title.

**Gated need: Career goals** — where the person wants to go, on what timeframe, and
what's driving the goal (promotion, pivot, skill-building, compensation, autonomy).
Keywords in Summary: career goals, aspirations, target role, direction, timeframe.

For each uncovered need, proceed to Step 3. If all needs are covered, there is
nothing to collect or register; go to Step 7 (Feedback).

## Step 3: Collect

For each uncovered need, follow one of two paths:

**Path A: Register a passed-in file.** If `$ARGUMENTS` contains a file path, use that
as the starting point for this need. Read it, confirm it covers the need, and
continue to collect any remaining uncovered needs. All collected files (passed-in and
interviewed) are registered together in Step 4.

**Path B: Guided interview.** For each uncovered need, run a brief interview to
collect the information. After the interview, write the response to a file with a
collision-checked name. Apply this collision-check logic:

1. Propose a default filename by convention (`context/career-background.md` for the
   background need, `context/career-goals.md` for the goals need).
2. Check if a file at that path already exists.
3. If it exists, generate a distinct filename by appending a `-N` suffix before the
   `.md` extension (try `-2`, then `-3`, etc.) until a filename that does NOT already
   exist is found. Never overwrite an existing context file.
4. Write to the final, collision-checked filename.

**Sample interview for career background:**

- What's your current role and industry?
- Roughly how many years of experience do you have, overall and in this role?
- What are your core responsibilities day to day?
- Thinking back over your career, what work has felt most satisfying? What has felt
  least satisfying, even when it went well on paper?

**Sample interview for career goals:**

- Where do you want to be in 1-2 years? In 5 years?
- What's driving that — promotion, a pivot, building a specific skill, compensation,
  more autonomy, something else?
- Is there a specific timeframe or deadline attached (e.g. a review cycle, a
  visa/relocation constraint, a personal milestone)?

## Step 4: Register

For each newly collected file, add a row to the `## Context files` table in the
appropriate `CLAUDE.md` (the root level, or the most specific nesting level that
applies). Use this format:

```markdown
| Label             | File                           | Summary                                                                              |
| ----------------- | ------------------------------ | ------------------------------------------------------------------------------------ |
| Career background | `context/career-background.md` | Current role, industry, experience, responsibilities, what's felt satisfying vs. not |
| Career goals      | `context/career-goals.md`      | Target direction, timeframe, and what's driving the goal                             |
```

The Summary column must be semantic and describe the need it covers — use keywords
that match the ones in Step 2 so future coverage checks will find it. Do **not**
hand-edit the **Key Config Files** table — the pre-commit hook owns that sync.

## Step 7: Feedback

Store learnings tagged `[cc-career:career-onboarding]` in `.claude/learnings.md`. For
example:

```
[cc-career:career-onboarding] user prefers "uplevel" over "promotion" when framing goals — 2026-08-18
[cc-career:career-onboarding] career-goals interview should ask about timeframe constraints up front, not as a follow-up — 2026-08-18
```

If the session revealed a rejected answer, path preference, or constraint that future
runs should skip, record it now.
