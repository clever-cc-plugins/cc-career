# Research Prompts

Ready-to-paste deep-research prompts to strengthen this plugin's skills with
scientific background and best practices, beyond what was available from
general knowledge when each skill was built.

## Scope

Only skills that apply a named framework to generate domain content get a
research prompt — a research prompt doesn't attach usefully to a pure
process/interview skill, since there's no framework to validate or deepen.

| Skill                        | Research prompt                                                | Why / why not                                                                                                |
| ---------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `career-plan`                | [career-plan.md](career-plan.md)                               | Applies Career Anchors + Skill-Will Matrix from general knowledge — no dedicated research backing yet        |
| `personal-branding-strategy` | [personal-branding-strategy.md](personal-branding-strategy.md) | Applies Golden Circle + Personal SWOT from general knowledge — no dedicated research backing yet             |
| `career-onboarding`          | —                                                              | Interview/setup process skill; no framework to research                                                      |
| `career-performance-review`  | —                                                              | Bet-verdict process skill (same shape as `cc-concept:strategy-performance-review`); no framework to research |

## Using a prompt

1. Open the prompt file for the skill you want to strengthen.
2. Paste the delimited prompt block into a "deep research" AI tool (Claude,
   ChatGPT, Gemini, Perplexity, or similar).
3. Save the response as Markdown under `docs/research/<skill-name>.md`
   (matching the convention `cc-concept` already uses for its researched
   skills).
4. Use the research to refine the skill's `_shared/*-frameworks.md` file —
   validate the frameworks already in use, add any well-evidenced ones the
   research surfaces, and note anything the current version got wrong.
