# cc-career: design

## Context

The clever-cc-plugins ecosystem has a marketing-focused strategy/execution split:
`cc-concept` (positioning, GTM, campaign concepting) produces planning artifacts;
`cc-content` (LinkedIn posts, blog articles, etc.) consumes them to produce finished
content. `cc-career` extends this into career development, playing the cc-concept
role but for the user's own career instead of a company's marketing.

Its sibling plugin, [`cc-coach`](https://github.com/clever-cc-plugins/cc-coach), plays
the cc-content role: live coaching conversations, including a `career-coach` skill
that consumes what cc-career produces. See cc-coach's own design doc for the
cross-plugin context-sharing mechanism that keeps the two decoupled (no install
dependency in either direction).

## Role

cc-career owns durable planning artifacts about the user's career: what the career
plan says, what personal brand they're building. It does not run coaching
conversations — that's cc-coach's `career-coach` skill, which reads cc-career's
output as optional context when present.

## v1 skill set

| Skill                        | Purpose                                                                                                                                                         |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `career-onboarding`          | Interview-driven setup of career context files (current role, goals, constraints). Entry point — other skills read what this produces.                          |
| `career-plan`                | Builds/refines a structured career plan or uplevel roadmap (skills to develop, milestones, timeline).                                                           |
| `personal-branding-strategy` | Defines the user's personal brand positioning (how they want to be seen, key themes). The artifact cc-content/cc-concept skills consume for LinkedIn posts etc. |
| `career-performance-review`  | Periodic review of progress against the career plan. Mirrors `content-performance-review` / `strategy-performance-review`.                                      |

## Testing and validation

No automated tests for skill content — consistent with cc-content and cc-concept,
since these are prompt-engineering artifacts, not code. Validation is the existing
`.githooks/pre-commit` (gitleaks + CLAUDE.md table sync) plus manual review against
the [repo guideline](https://github.com/clever-cc-plugins/marketplace/blob/main/docs/cc-plugin-repo-guideline.md)'s
checklist.

## Out of scope for this design

- The exact SKILL.md content for each skill (written during implementation, following
  existing skills of the same shape as templates — e.g. `career-plan` follows
  `gtm-plan`'s structure from cc-concept).
- Registering this plugin in the marketplace catalog (`marketplace.json`) — deferred
  until skills exist.
