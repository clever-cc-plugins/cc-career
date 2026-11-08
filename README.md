<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/clever-cc-plugins/.github/main/assets/logo-dark.svg" />
    <img src="https://raw.githubusercontent.com/clever-cc-plugins/.github/main/assets/logo.svg" width="220" alt="clever [cc] plugins" />
  </picture>
</p>

# <img src="assets/icon.svg" width="40" height="40" align="top" alt="" />&nbsp;cc-career

A [Claude Code](https://claude.ai/code) plugin for career planning, upleveling, and personal-branding strategy. It does for your career what `cc-concept` does for a company's marketing: it builds the strategic frame first. Your career is the one legacy codebase you can't rewrite from scratch, so it's worth having a plan for it.

---

## At a Glance

`cc-career` owns durable planning artifacts about your career: your career plan and the personal brand you're building. It doesn't run coaching conversations. That's the job of its sibling plugin [`cc-coach`](https://github.com/clever-cc-plugins/cc-coach) and its `career-coach` skill, which reads what `cc-career` produces as optional context.

| Skill                                   | What it does                                                                                                                                |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `/cc-career:career-onboarding`          | Interviews you about your career context (current role, industry, goals, constraints). It's the entry point, since the other skills read it |
| `/cc-career:career-plan`                | Builds or refines a structured career plan or uplevel roadmap: direction, prioritized skills to develop, and a milestone timeline           |
| `/cc-career:personal-branding-strategy` | Defines your personal brand (Why, How, What), a personal SWOT, and content themes that the `cc-content` and `cc-concept` skills consume     |
| `/cc-career:career-performance-review`  | Reviews your career plan's milestones as hit, missed, or in progress, and recommends what to feed into the next planning round              |

---

## Installation

Open Claude Code in any project and run:

```
/plugin marketplace add clever-cc-plugins/marketplace
/plugin install cc-career@clever-cc-plugins
```

All skills are available immediately.

### Keeping Skills Current

Auto-update is off by default for third-party marketplaces. To switch it on:

1. Run `/plugin` in Claude Code
2. Open the **Marketplaces** tab
3. Toggle auto-update for `clever-cc-plugins/marketplace`

Once it's on, Claude Code updates the skills on startup whenever a new version is available.

### Uninstalling

```
/plugin uninstall cc-career@clever-cc-plugins
```

To remove the marketplace as well (this uninstalls every plugin you installed from it):

```
/plugin marketplace remove clever-cc-plugins
```

---

## Getting Started

**Step 1: Run onboarding**

```
/cc-career:career-onboarding
```

This populates `context/` with your current role, industry, experience, and goals. The other skills read this context automatically.

**Step 2: Build a plan**

```
/cc-career:career-plan
```

You get a career direction, prioritized skills to develop, and a milestone timeline with review triggers, so "I'll look at this again someday" gets an actual date.

**Step 3: Define your personal brand (optional)**

```
/cc-career:personal-branding-strategy
```

This produces the Why, How, and What plus the content themes that `cc-content` and `cc-concept` skills draw on when they write on your behalf.

**Step 4: Review progress periodically**

```
/cc-career:career-performance-review
```

The review marks each milestone of your career plan as hit, missed, or still in progress, and feeds the result into your next planning round.

---

## Working with cc-coach

Installing [`cc-coach`](https://github.com/clever-cc-plugins/cc-coach) is optional. Its `career-coach` skill runs a standalone GROW-model session by default. If it finds a `career-plan.md` or `personal-branding-strategy.md` registered in your project's `## Context files` table (both are artifacts that this plugin produces), it coaches against that specific plan instead of the generic framework. Neither plugin needs the other.

---

## Contributing

Issues and pull requests are welcome. If you've spotted a gap or a rough edge, please open an issue first so we can agree on the approach.

---

## License

[MIT](LICENSE), Copyright (c) 2026 Michael van Laar

---

<p align="center">
  Part of the <a href="https://github.com/clever-cc-plugins">clever-cc-plugins</a> family · <a href="https://github.com/clever-cc-plugins/marketplace">marketplace</a> · <a href="https://github.com/clever-cc-plugins/cc-config">cc-config</a> · <a href="https://github.com/clever-cc-plugins/cc-concept">cc-concept</a> · <a href="https://github.com/clever-cc-plugins/cc-content">cc-content</a> · <a href="https://github.com/clever-cc-plugins/cc-coach">cc-coach</a> · <a href="https://github.com/clever-cc-plugins/cc-handoff">cc-handoff</a> · <a href="https://github.com/clever-cc-plugins/cc-chime">cc-chime</a> · <a href="https://github.com/clever-cc-plugins/cc-chef">cc-chef</a>
</p>
