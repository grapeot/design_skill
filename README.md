# design_skill

A structured UI design judgment skill for AI coding agents. Teaches agents how to evaluate, critique, and improve user interfaces across iOS, Android, and Web — not by adding more design knowledge, but by transferring the evaluation criteria that expert designers use.

## What this is

A single Markdown skill file that gives AI coding agents a design review loop: define what success looks like before generating UI, verify with real artifacts (screenshots, builds, accessibility scans) after implementation, and capture reusable design lessons over time.

## What this is not

- A design tool or Figma replacement
- A universal "make it prettier" prompt
- A design system or component library
- An Anthropic plugin (this is tool-agnostic, filesystem-native)

## Inspiration

Inspired by Anthropic's open-source [Design Plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/design) (design critique, handoff, accessibility, UX copy, design system audit, research synthesis) and [Frontend Design Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design) (aesthetic direction). For a detailed analysis of the design philosophy behind these plugins, see [Claude Design Reverse Engineering](https://yage.ai/share/claude-design-reverse-engineering-20260607.html).

Key ideas absorbed:

- **Evaluation-first**: give the agent a framework for judging "what counts as good" before asking it to generate
- **Criteria transfer**: the model already knows design principles — what it needs is the operational structure of critique (first impression → usability → visual hierarchy → consistency → accessibility)
- **Progressive disclosure**: keep the root skill short; load platform-specific rules and references only when needed
- **Deterministic tools supply facts, model supplies judgment**: use CLI tools for contrast ratio, touch targets, view hierarchy, accessibility scans; reserve LLM judgment for task coherence, visual emphasis, copy quality

Key differences from Anthropic's approach:

| Claude Design | design_skill |
|---|---|
| Figma/Web-centric | iOS, Android, Web |
| Requires plugin runtime (`~~connector` placeholders) | Filesystem-native (paths, not placeholders) |
| Designed for team design workflow | Designed for solo builder + AI agent |
| Six separate SKILL.md files | Single root skill; platform rules loaded on demand |
| Anthropic-exclusive tool ecosystem | Tool-agnostic; references existing platform CLI/XCTest/Playwright |

## Installation

This is a loose Markdown-based skill. To use it, copy `skills/design_skill.md` into your AI coding tool's skills directory, or symlink it.

### For OpenCode / Claude Code / Codex / Cursor

Tell your AI agent:

> Install the design_skill from https://github.com/grapeot/design_skill — read skills/design_skill.md, symlink it into my workspace's rules/skills/ directory, and update the skills INDEX if one exists.

The agent should:
1. Clone or reference the repo
2. Add the skill path to the workspace discovery chain (INDEX.md, AGENTS.md, or CLAUDE.md)
3. Not copy the skill inline — keep it as a referenced file for easy updates

## Project Structure

- `skills/design_skill.md` — root skill (the primary artifact)
- `skills/design_critique.md` — sub-skill: structured design critique reference
- `skills/design_system.md` — sub-skill: design system audit reference
- `docs/prd.md` — product requirements and design rationale
- `docs/rfc.md` — architecture, design decisions, trade-offs
- `docs/working.md` — changelog and lessons learned
- `docs/test.md` — test strategy and acceptance criteria

## Privacy

This repository is designed to be publishable with only fake examples. No real credentials, internal paths, or private data are included.

## License

MIT
