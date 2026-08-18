# cc-career Skill Contract — Shared Reference

Every cc-career skill follows the same step sequence. A SKILL.md **references this
file** rather than restating the sequence — this is the single source of truth, so
the steps cannot drift skill to skill. A skill documents only its own specifics:
what it gates on, what it generates, and where it writes.

## The sequence

0. **Recall learnings** (silent) — read `.claude/learnings.md` if present, across
   all plugin tags. Apply relevant observations. Never announce this step; surface
   nothing unless it changes behavior.
1. **Load context** — read the `## Context files` table in `CLAUDE.md`. Match needs
   against the **Summary** column semantically — never against a label or filename.
2. **Gating check** — confirm the skill has the context it needs. Warn only on the
   _semantic absence_ of a required content-need, never on a missing label.
3. **Calibrate** — set depth and scope from the request and the context available.
4. **Generate** — produce the artifact against the relevant `_shared/` reference.
5. **Internal quality check** — self-review before showing anything.
6. **Delimited output** — return the artifact inside clear delimiters.
7. **Two-phase feedback** — auto-store lightweight learnings (tagged
   `[cc-career:<skill-name>]`); ask before promoting anything heavier.
