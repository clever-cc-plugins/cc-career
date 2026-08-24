<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/clever-cc-plugins/.github/main/assets/logo-dark.svg" />
    <img src="https://raw.githubusercontent.com/clever-cc-plugins/.github/main/assets/logo.svg" width="220" alt="clever [cc] plugins" />
  </picture>
</p>

# cc-career

A [Claude Code](https://claude.ai/code) plugin for career planning, upleveling, and personal-branding strategy — the cc-concept role, but for your own career instead of a company's marketing.

---

## Plugin: `cc-career`

`cc-career` owns durable planning artifacts about your career: what your career plan says, what personal brand you're building. It doesn't run coaching conversations — that's its sibling plugin [`cc-coach`](https://github.com/clever-cc-plugins/cc-coach)'s `career-coach` skill, which reads what `cc-career` produces as optional context when it's installed.

| Skill                                   | What it does                                                                                                                              |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `/cc-career:career-onboarding`          | Interview-driven setup of career context (current role, industry, goals, constraints). Entry point — other skills read what this produces |
| `/cc-career:career-plan`                | Builds or refines a structured career plan or uplevel roadmap: direction, prioritized skills to develop, and a milestone timeline         |
| `/cc-career:personal-branding-strategy` | Defines your personal brand Why/How/What, a personal SWOT, and content themes — the artifact `cc-content`/`cc-concept` skills consume     |
| `/cc-career:career-performance-review`  | Reviews a career plan's milestones as hit, missed, or in progress, and recommends what to feed into the next planning round               |

---

## Installation

Open Claude Code in any project and run:

```
/plugin marketplace add clever-cc-plugins/marketplace
/plugin install cc-career@clever-cc-plugins
```

This makes all skills immediately available.

### Keeping Skills Current

Auto-update is disabled by default for third-party marketplaces. To enable it:

1. Run `/plugin` in Claude Code
2. Go to the **Marketplaces** tab
3. Toggle auto-update for `clever-cc-plugins/marketplace`

Once enabled, skills update automatically on startup when new versions are available.

---

## Getting Started

**Step 1 — Run onboarding**

```
/cc-career:career-onboarding
```

This populates `context/` with your current role, industry, experience, and goals. The other skills read this context automatically.

**Step 2 — Build a plan**

```
/cc-career:career-plan
```

Produces a career direction, prioritized skills to develop, and a milestone timeline with review triggers.

**Step 3 — Define your personal brand (optional)**

```
/cc-career:personal-branding-strategy
```

Produces the Why/How/What and content themes that `cc-content` and `cc-concept` skills draw on when writing on your behalf.

**Step 4 — Review progress periodically**

```
/cc-career:career-performance-review
```

Verdicts which milestones from your career plan were hit, missed, or are still in progress, and feeds the result into your next planning round.

---

## Working with cc-coach

Installing [`cc-coach`](https://github.com/clever-cc-plugins/cc-coach) is optional, not required. Its `career-coach` skill runs a standalone GROW-model coaching session by default; if it finds a `career-plan.md` or `personal-branding-strategy.md` registered in your project's `## Context files` table — the artifacts this plugin produces — it coaches against that specific plan instead of the generic framework. Neither plugin depends on the other being installed.

---

## License

[MIT](LICENSE) — Copyright (c) 2026 Michael van Laar

---

<p align="center">
  Part of the <a href="https://github.com/clever-cc-plugins">clever-cc-plugins</a> family · <a href="https://github.com/clever-cc-plugins/marketplace">marketplace</a> · <a href="https://github.com/clever-cc-plugins/cc-config">cc-config</a> · <a href="https://github.com/clever-cc-plugins/cc-concept">cc-concept</a> · <a href="https://github.com/clever-cc-plugins/cc-content">cc-content</a> · <a href="https://github.com/clever-cc-plugins/cc-coach">cc-coach</a> · <a href="https://github.com/clever-cc-plugins/cc-handoff">cc-handoff</a> · <a href="https://github.com/clever-cc-plugins/cc-chime">cc-chime</a>
</p>
