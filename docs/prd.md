# PRD: design_skill

## Summary

`design_skill` gives AI coding agents a structured framework for UI design judgment. It teaches agents how to evaluate, critique, and improve interfaces — not by injecting more design knowledge into the context window, but by providing the evaluation criteria that expert designers use when they judge a screen.

## Problem

AI coding agents can generate UI. They know what a button is, what contrast is, what visual hierarchy means. But when asked "does this screen work?", they default to vague praise or generic checklist items ("the CTA could be more prominent", "consider adding more whitespace"). The output reads like a junior designer who knows the vocabulary but hasn't developed judgment.

Anthropic's Design Plugin solves this by decomposing design work into six focused skills (critique, design system, handoff, UX copy, accessibility, research synthesis), each with its own rubric, output format, and connector slots. This works well inside Claude's plugin runtime but is vendor-locked to Anthropic's ecosystem.

The problem for everyone else: AI agents can make UI, but they can't reliably judge UI. Every attempt to add design quality ends up as either a vague prompt suffix ("make it look good") or a bloated checklist that the model can recite but can't apply.

## Target Users

- **Builders using AI coding agents** who need their agent to produce or judge UI for iOS, Android, or Web
- **Solo developers and small teams** who don't have a dedicated designer but need their agent to catch visual and interaction problems
- **AI agent maintainers** who want to add design judgment as a reusable capability in their context infrastructure

## Success Criteria

1. An agent equipped with this skill produces a design critique that a human designer would recognize as structured, specific, and actionable — not a generic "looks nice" or a vague checklist
2. The skill works across iOS, Android, and Web without requiring platform-specific reconfiguration
3. The skill fails gracefully: when artifacts (screenshots, builds) are unavailable, it acknowledges the limit and gives directional guidance rather than pretending certainty
 4. The root skill stays under ~220 lines, with specialized judgment frameworks extracted into on-demand sub-skills
 5. A new user can install the skill by telling their AI agent one sentence

## Non-Goals

- Replacing Figma or any design tool
- Providing a universal aesthetic standard (taste is contextual)
- Supporting team design workflows, design system management, or Figma-to-code pipelines
- Matching Claude Design's product feature set (canvas, inline refinement, handoff bundle)

## Inspiration and Differences

### What we absorbed from Claude Design / Frontend Design plugins

- **Evaluation-first**: define what "good" means before asking the model to generate. The model is a reasoning engine; it needs the operational structure of critique, not more knowledge about design.
- **Criteria transfer**: the plugin gives the model a rubric (first impression → usability → visual hierarchy → consistency → accessibility) rather than deeper design knowledge. The model already knows what good design looks like — it just didn't know how to apply that knowledge systematically.
- **Progressive disclosure**: keep the root skill short. Platform rules, accessibility references, and edge-case catalogs are loaded on demand, not baked into every prompt.
- **Cognitive decomposition**: critique, accessibility audit, and UX copy are different judgment modes. Mixing them in one prompt degrades all three. Each sub-task needs its own clean evaluation standard.
- **Aesthetic direction via constraint, not instruction**: the Frontend Design plugin doesn't say "be creative." It forces the model to pick a conceptual direction (brutalist minimalism, maximalist chaos, organic natural) and execute every design choice to serve that direction. This prevents regression to the mean.

### Key differences from Anthropic's approach

1. **Multi-platform, not Figma-centric.** Claude Design assumes a Figma → Web pipeline. Our skill handles iOS (SwiftUI, UIKit), Android (Jetpack Compose, XML), and Web (HTML/CSS/JS, React, Tailwind) as first-class targets. Platform-specific rules live in separate reference files, loaded only when relevant.

2. **Filesystem-native, no plugin runtime.** Claude Design uses `~~design tool` placeholders resolved at runtime by Anthropic's plugin system. Our skill uses file paths and discovery conventions. If a project has `DESIGN.md`, screenshots, or a simulator build, the agent finds them by path. If not, it degrades gracefully.

3. **Single root skill, not six separate SKILL.md files.** Anthropic's plugin system auto-loads the right skill for the right task. In a file-system-based skill system, loading six files before each task is heavy. Our root skill is one file that routes to platform references on demand. Sub-skills may be extracted later if usage patterns justify it.

4. **Evidence-based QA, not just critique.** Claude Design's critique is a text artifact. Our skill requires the agent to verify judgments against real artifacts: simulator screenshots, accessibility scans, build/test results. Judgment without evidence is opinion; with evidence, it's auditable.

5. **Explicit stop conditions.** Our skill defines when the agent should refuse to make a design judgment: no artifacts available, no target platform identified, destructive operations involved, or no clear success criterion. Anthropic's plugins assume the user knows when to stop.

## Scope

The root skill (`skills/design_skill.md`, ~220 lines) defines:

1. A Phase 0 request classifier that determines the type of design work needed before entering the review loop
2. A design review loop: design intent → context pull → aesthetic direction → implementation contract → evidence-based QA → lesson capture
3. An evaluation-first entry: before producing or judging UI, the agent must articulate what success looks like for this specific task
4. Platform routing that points to platform-specific rules without bundling them
5. Stop/refusal conditions that prevent the agent from pretending certainty without evidence
6. A UX Copy rubric for CTAs, error messages, empty states, confirmation dialogs, and tone
7. A mandate to verify with real artifacts (screenshots, builds, accessibility scans) rather than matching pixels against a mental template

Specialized judgment frameworks (design critique, design system audit) are extracted into sub-skills loaded on demand via progressive disclosure.
