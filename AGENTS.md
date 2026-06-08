# AGENTS.md — design_skill

Agent-readable instruction file for the `design_skill` project. This file is the first thing any AI agent reads when entering this repo.

## Project Identity

`design_skill` provides a structured UI design judgment skill for AI coding agents. It teaches agents how to evaluate, critique, and improve user interfaces across iOS, Android, and Web — not by adding more design knowledge, but by transferring the evaluation criteria that expert designers use.

Inspired by Anthropic's [Design Plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/design) and [Frontend Design Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design), but re-architected for:

- **Multi-platform** (iOS, Android, Web, not just Figma-to-Web)
- **File-system-native** (no plugin runtime required; uses paths, not `~~connector` placeholders)
- **Evaluation-first** (judgment before generation; evidence-based QA)
- **Platform-specific verification** (simulator screenshots, accessibility tools, build/test results)

## Project Structure

```
design_skill/
├── AGENTS.md              ← this file
├── .gitignore
├── .env.example
├── README.md              ← public-facing readme
├── skills/
│   ├── design_skill.md    ← root skill (~190 lines, the primary artifact)
│   ├── design_critique.md ← sub-skill: structured design critique reference
│   └── design_system.md   ← sub-skill: design system audit reference
├── docs/
│   ├── prd.md             ← product requirements: what, who, why
│   ├── rfc.md             ← architecture, design decisions, trade-offs
│   ├── working.md          ← changelog + lessons learned
│   └── test.md            ← test strategy and acceptance criteria
```

## Critical Constraints

1. **This is a public repo.** All files must use only fake examples. No real emails, API keys, internal paths, 1Password vault references, or private data.
2. **The root skill is the primary artifact.** Everything else exists to explain, justify, and maintain the skill. The root skill file itself should stay under ~200 lines. Do not inflate it — use sub-skills for specialized content.
3. **Sub-skills are loaded on demand.** Sub-skills (`design_critique.md`, `design_system.md`) are plain-text reference files, not secondary entry points. The root skill routes to them; agents load them only when the task matches their scope.
4. **English only.** All docs, code comments, and the skill file itself are in English.
5. **Working.md discipline.** After every change, append to `docs/working.md` — a dated changelog entry plus any lessons learned.

## Environment

- No Python/Node dependencies required for the skill itself. It is a plain Markdown file.
- The skill references platform tools (Xcode, Android SDK, Playwright) but does not bundle them. Tool availability is checked at runtime with graceful degradation.

## Git Conventions

- Branch: `master` (not `main`)
- Commit messages: `<verb>: <description>`. Examples: `add: initial MVP skill`, `docs: prd and rfc`, `refine: platform routing logic`
- Never commit `.env`, `.venv`, or any file containing real credentials.
