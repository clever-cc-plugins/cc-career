# cc-career

A Claude Code plugin for career planning, upleveling, and personal-branding strategy.

Status: v1 skills implemented, not yet registered in the marketplace catalog. See
`docs/superpowers/specs/2026-08-18-cc-career-design.md` for the design.

## Key Config Files

| File                                                           | Purpose                                                                    |
| -------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `.claude/format-markdown.sh`                                   | PostToolUse hook: formats Markdown files with prettier after edits         |
| `.claude/guard-secret-files.sh`                                | PreToolUse hook: blocks reads/edits/writes of secret .env files            |
| `.claudeignore`                                                | Paths excluded from Claude Code indexing                                   |
| `CLAUDE.md`                                                    | Project instructions, loaded every message                                 |
| `.claude/settings.json`                                        | Permissions, hooks, environment variables                                  |
| `.githooks/pre-commit`                                         | Secret scanning (gitleaks) + CLAUDE.md table sync                          |
| `.gitignore`                                                   | Git ignore patterns                                                        |
| `plugins/cc-career/.claude-plugin/plugin.json`                 | Plugin manifest                                                            |
| `plugins/cc-career/skills/career-onboarding/SKILL.md`          | Skill: Collect career background and goals context via interview           |
| `plugins/cc-career/skills/career-performance-review/SKILL.md`  | Skill: Verdict career-plan milestones as hit, missed, or in progress       |
| `plugins/cc-career/skills/career-plan/SKILL.md`                | Skill: Build a career plan (Career Anchor, prioritized skills, milestones) |
| `plugins/cc-career/skills/personal-branding-strategy/SKILL.md` | Skill: Define personal brand Why/How/What, SWOT, and content themes        |
| `scripts/sync-config-table.sh`                                 | Keeps Key Config Files table in sync on each commit                        |

## Context files

Project-scope context files registered by cc-career skills. Register new context
files by running `/career-onboarding` or by adding them manually to this table.

| Label                      | Path                                    | Summary                                                                              |
| -------------------------- | --------------------------------------- | ------------------------------------------------------------------------------------ |
| Career background          | `context/career-background.md`          | Current role, industry, experience, responsibilities, what's felt satisfying vs. not |
| Career goals               | `context/career-goals.md`               | Target direction, timeframe, and what's driving the goal                             |
| Career plan                | `context/career-plan.md`                | Career direction, Career Anchor, prioritized skills, and milestone timeline          |
| Personal branding strategy | `context/personal-branding-strategy.md` | Personal brand Why/How/What, Personal SWOT, and content themes                       |
| Career performance review  | `context/career-performance-review.md`  | Verdicts on which career-plan milestones were hit, missed, or in progress            |

## References

@docs/superpowers/specs/2026-08-18-cc-career-design.md **Read when:** planning or
implementing skills for this plugin

## Conventions

- Follow the [cc-plugin-repo-guideline](https://github.com/clever-cc-plugins/marketplace/blob/main/docs/cc-plugin-repo-guideline.md) for all structural decisions
- Skill names match their directory name (kebab-case)

## Don't

- Don't commit secrets or credentials to git
- Don't use `--force` flags — fix the underlying issue instead
- Don't copy skill files into the clever-cc-plugins umbrella repo — it references them via `git-subdir`

## Learnings

When the user corrects a mistake or points out a recurring issue, append a one-line
summary to `.claude/learnings.md`. Don't modify `CLAUDE.md` directly.

## Compact Instructions

When compacting, preserve: list of modified files, current test status, open TODOs, and key decisions made.
