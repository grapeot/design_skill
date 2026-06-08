# RFC: design_skill Architecture

## Overview

This RFC describes the architecture of `design_skill` — a filesystem-native skill family for UI design judgment in AI coding agents. The skill is designed to work as loose Markdown files with no runtime dependencies, no plugin system, and no vendor lock-in.

## Design Principles

### 1. Evaluation-first

The agent must define what success looks like before generating or judging UI. This is not a polish step — it is the entry condition. Without a clear success criterion, the agent should refuse to proceed rather than generate generic output.

Concretely: the skill requires the agent to articulate "this screen needs to win at ______" — exactly one primary goal — before any design work begins.

### 2. Criteria transfer, not knowledge injection

The model already knows design principles (contrast, hierarchy, consistency, accessibility, platform conventions). What it lacks is the operational structure of applying them systematically. The skill provides that structure: a fixed sequence of judgment dimensions, each with specific questions and a stop condition for insufficient information.

### 3. Progressive disclosure

The root skill (~220 lines) defines the agent's role, the Phase 0 request classifier, the judgment loop, UX copy rubric, and routing rules. Platform-specific rules (iOS HIG, Material Design, Web accessibility), anti-examples, and tool references live in separate files. The agent loads them only when the task requires a specific platform or judgment dimension.

### 4. Deterministic tools supply facts, model supplies judgment

CLI tools (contrast checkers, accessibility scanners, build/test runners, screenshot tools) provide facts. The model provides interpretation. The skill must never ask a CLI tool to produce a judgment ("rate this design 1-10"). Tools measure; models decide.

### 5. Graceful degradation

When tools are unavailable (no Xcode, no Android SDK, no browser automation), the skill degrades to heuristic-only judgment and explicitly acknowledges the limitation. It never pretends certainty without evidence.

## Architecture

### File Layout

```
design_skill/
└── skills/
    ├── design_skill.md     ← root skill (~240 lines, consumed at runtime)
    ├── design_critique.md   ← sub-skill: structured critique reference
    ├── design_system.md     ← sub-skill: design system audit reference
    └── frontend_design.md   ← sub-skill: Web/frontend aesthetic direction
```

The skill file has these sections:

1. **When to use** — trigger conditions
2. **Your role** — agent as design guide, not UI polisher
3. **Phase 0** — request classification (visual polish, UX redesign, new feature, design QA, unclear)
4. **Stop conditions** — when to refuse or degrade
5. **The design review loop** — the six-step cycle
6. **UX Copy** — inline rubric for CTAs, errors, empty states, confirmation dialogs, tone
7. **Related sub-skills** — pointers to design_critique.md, design_system.md, and frontend_design.md
8. **Platform routing** — how to find and load platform-specific rules
9. **Quality gates** — what must be true before the agent can claim "done"

### The Design Review Loop

```
┌─────────────────────────────────────────────────────┐
│ 1. DESIGN INTENT                                     │
│    What must this screen win at? (one thing)         │
│    What platform? What user task?                    │
│    STOP if: no clear answer → ask, don't guess       │
├─────────────────────────────────────────────────────┤
│ 2. CONTEXT PULL                                      │
│    Find: existing screenshots, DESIGN.md, code,      │
│    platform rules, anti-examples                     │
│    DEGRADE if: nothing found → acknowledge, proceed  │
├─────────────────────────────────────────────────────┤
│ 3. AESTHETIC DIRECTION                               │
│    Pick a conceptual direction (not "clean/modern")  │
│    State what this direction prohibits               │
│    DEGRADE if: task is pure bug fix → skip this      │
├─────────────────────────────────────────────────────┤
│ 4. IMPLEMENTATION CONTRACT                           │
│    States, breakpoints, copy, accessibility,         │
│    edge cases, verification screenshots              │
│    Only required when agent is writing code          │
├─────────────────────────────────────────────────────┤
│ 5. EVIDENCE-BASED QA                                 │
│    Run: screenshot/build → inspect → compare to      │
│    intent. Deterministic checks first, then          │
│    model judgment.                                   │
│    STOP if: destructive action, missing platform     │
├─────────────────────────────────────────────────────┤
│ 6. CAPTURE LESSON                                    │
│    What reusable design judgment did we learn?       │
│    (Optional per-task; accumulates across sessions)  │
└─────────────────────────────────────────────────────┘
```

### Platform Routing

The root skill does not contain platform-specific rules. It contains routing instructions:

```
If iOS → look for ios_hig.md or use heuristic iOS knowledge
If Android → look for material_rules.md or use heuristic Android knowledge
If Web → look for web_rules.md or use heuristic Web knowledge
If none specified → ask
```

The agent is expected to find these reference files in the project or workspace. If not found, it uses its training knowledge of platform conventions (which is generally sufficient for common cases) and acknowledges the heuristic limitation.

### Quality Gates

The skill defines three gates that must be satisfied before the agent can claim a design judgment is complete:

- **Gate 1 (Artifact gate):** Is there at least one real artifact (screenshot, running build, Figma link)? If not, output is labeled as "directional guidance only."
- **Gate 2 (Platform gate):** Is the target platform identified? If not, platform-specific rules are skipped and the output is labeled as "platform-agnostic."
- **Gate 3 (Intent gate):** Is there a clear design intent? If not, the agent must ask for one before proceeding.

### Stop/Refusal Conditions

The skill explicitly defines when to refuse to make a judgment:

1. No artifacts or screenshots → can give directional guidance, not specific critique
2. Destructive action (delete, payment, privacy, auth, medical) → elevate confirmation and copy review
3. No clear design intent → ask, don't guess
4. Before/after comparison without a baseline → cannot claim improvement

## Future Expansion

### When to extract sub-skills

A sub-skill should be extracted from the root only when all three conditions are met:
1. The judgment dimension has been used in 3+ real tasks
2. Its rules are long enough (>30 lines) to noticeably pollute the root skill
3. There is a clear, non-overlapping scope (e.g., "accessibility audit" vs "visual hierarchy critique")

Candidates for future sub-skills, in priority order:
1. `ui_design_anti_examples.md` — catalog of real failure patterns with before/after

### What should NOT become a sub-skill

- `evaluation_first.md` — too generic; this principle belongs in meta-skills, not design
- `connector_slot.md` — filesystem-native approach makes this unnecessary
- `latent_vs_deterministic.md` — philosophical distinction, not an operational skill
- Per-component skills (`landing_page.md`, `dashboard.md`) — too narrow for a global skill

## Trade-Offs

### Single file vs skill family

**Chosen: single root with on-demand sub-skills.** The root skill (~220 lines) defines the agent role, Phase 0 request classifier, review loop, stop conditions, UX copy rubric, platform routing, and quality gates. Specialized judgment frameworks (design critique, design system audit) live as separate plain-text reference files loaded on demand. This keeps the root skill focused while allowing sub-skills to provide depth when needed.

Sub-skills are extracted when the judgment dimension has clear scope boundaries and enough content to noticeably pollute the root skill. The extraction is conservative: critique, design system audit, and frontend aesthetic direction are plain-text files, not secondary entry points. The root skill explicitly lists when to load each one.

### Installation model

**Chosen: install the repo or complete `skills/` directory, expose only the root skill.** Earlier installation guidance treated `skills/design_skill.md` as a standalone file that could be symlinked globally. That no longer matches the architecture: the root skill now routes to sibling sub-skills, so a single-file symlink can strand the references.

Installers should clone or vendor the repository in a stable location, then add the root skill to the workspace discovery chain. If the target system requires copying files into a skills directory, copy the complete `skills/` directory and expose only `design_skill.md` in any global index. Do not symlink every sub-skill globally; the root skill owns routing.

### Platform-agnostic vs platform-specific

**Chosen: agnostic root with routing.** A platform-agnostic skill would be too vague to be useful. A platform-specific skill would need three variants (iOS, Android, Web) which multiplies maintenance cost. The routing approach keeps the root skill universal while allowing platform details to live in reference files that users can customize.

### Tool-dependent vs heuristic-only

**Chosen: tool-preferred, heuristic-degraded.** Requiring tools (Xcode, Android SDK, Playwright) would make the skill unusable in many environments. Being purely heuristic would reduce its outputs to opinion. The degradation model — try tools, fall back to heuristics, label the output accordingly — preserves usefulness across environments while rewarding environments with proper tooling.

## References

- [Claude Design Reverse Engineering](https://yage.ai/share/claude-design-reverse-engineering-20260607.html) — analysis of Anthropic's design plugin architecture
- [Cursor Agent Harness: Evaluation-First](https://yage.ai/share/cursor-agent-harness-evaluation-first-20260501.html) — evaluation-first methodology in agent products
- [Thin Harness, Fat Skills](https://yage.ai/share/thin-harness-fat-skills-20260414.html) — Garry Tan's framework for skill architecture
- Anthropic's [Design Plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/design) and [Frontend Design Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design) — source of inspiration
