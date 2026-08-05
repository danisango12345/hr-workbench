# hr-workbench

A personal Cowork/Claude Code plugin for HR business-partner work. Bundles eight skills that help run 1:1s, hold people to their commitments, prepare for leader conversations, research policy and market-practice questions, explain things in plain language, and keep AI-built tools inside sensible guardrails.

## Skills

| Skill | What it does |
|-------|--------------|
| `1-on-1` | Prepare for and capture 1:1s using a shared running doc, built around blockers, goals and development. |
| `chase` | Track what people owe you and what you owe them; help hold people to what they agreed. |
| `prep-partnering` | Prepare for and capture conversations with the business leaders you partner with. Notes stay private. |
| `build-rules` | Eight rules for deciding whether to build/automate an HR or ops task, and how it must behave. |
| `research` | Research an HR or business-ops question across documents, law, and market data, grading every claim by source quality. |
| `ask-before-producing` | Pause at the shift from gathering to producing and confirm the person is ready. |
| `verification-before-completion` | Require evidence before claiming something is done, correct, or compliant. |
| `eli5` | Explain a site, screenshot or snippet in plain, jargon-free language, tailored for a named audience. |

## Install

Add this repo as a plugin marketplace, then install `hr-workbench` from it.

## Updating

1. Edit or add skills under `skills/`.
2. Bump `version` in both `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`.
3. `git add . && git commit -m "..." && git push`.
4. Update the plugin on your side — the higher version triggers the sync.

## Versioning

Semantic versioning: `MAJOR.MINOR.PATCH`. Adding a skill is usually a MINOR bump; a small fix is a PATCH. Any increase over the previously synced version triggers a re-pull.
