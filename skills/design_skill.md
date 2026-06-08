# UI Design Judgment

Give AI coding agents a structured framework for evaluating, critiquing, and improving user interfaces across iOS, Android, and Web. Inspired by Anthropic's open-source [Design Plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/design) and [Frontend Design Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design), re-architected for multi-platform, filesystem-native, evaluation-first workflows.

## When to use

Trigger when the user asks you to:
- Review, critique, or audit an existing UI
- Generate a new screen, page, or interface
- Improve visual quality, usability, or platform feel
- Evaluate accessibility or interaction design
- Prepare design specs for handoff

## Core principle: product function before visual form

Design judgment must start from what the product does, not how it looks. A UI that appears sophisticated on first glance but leaves the user confused about what to do next has failed, regardless of its visual polish. The visual language serves the task — not the other way around.

When you are asked to "improve the design," your first question must be about the user's task, not the color palette. When you are asked to generate a new screen, you must understand what the user needs to accomplish before you decide what it should look like.

Consequence of ignoring this: the most common failure mode of AI-assisted design is a UI that reads as "advanced" (dark backgrounds, glass morphism, bold typography, dramatic spacing) but scores poorly on task completion. Visual sophistication and task coherence are separate dimensions. Optimize for the second; let the first follow.

## Stop conditions

Before doing any design work, check these. If any fail, refuse and explain why.

- **No artifacts**: no screenshot, running build, Figma link, or live URL → give directional guidance only. Never claim pixel-level certainty without evidence.
- **No platform**: target platform not identified (iOS? Android? Web?) → ask before proceeding.
- **No intent**: user hasn't stated what this screen needs to accomplish → ask: "What one thing must this screen win at?"
- **Visual polish request without task context**: user asks to "make it look better" or "modernize the design" without describing what the user needs to do → push back: "What user task is this screen failing at today? What should the user do, know, or feel after using this screen that they don't today?"
- **Destructive action**: delete, payment, auth, privacy, medical, financial → elevate confirmation copy review and explain risks before touching UI.

## The review loop

Follow this sequence. Don't skip steps. If a step can't be completed due to missing information, degrade explicitly rather than pretending.

### 1. Design intent

Before looking at any UI, state what success means for this specific task. Exactly one primary goal, expressed as a user outcome — what the user should be able to do, know, or decide after interacting with this screen. The intent must be about product function, not visual quality. Examples:

- "This onboarding must get the user to the first real action in under 30 seconds."
- "This error state must reduce panic so the user reads the recovery instructions."
- "This dashboard must let an operator spot anomalies in one scan."

Not: "make it look modern" or "improve the UX." Those aren't goals — they're symptoms of not having one. Not: "create a beautiful landing page." Beauty is not an outcome; it is a byproduct of coherent choices serving a clear function.

If you cannot articulate the intent, do not proceed. An AI tool given only visual instructions will optimize for visual impact at the expense of function — the most common failure mode in AI-assisted design.

### 2. Context pull

Gather what already exists before suggesting anything new. Check these sources in order:

1. Project-level design direction: `DESIGN.md`, `AGENTS.md`, brand guidelines, design system docs
2. Existing UI artifacts: screenshots, running app, Figma links, Storybook, build output
3. Platform conventions: iOS HIG, Material Design, Web accessibility standards (from your training data or workspace reference files)
4. Historical anti-patterns: check `rules/design/anti_examples.md` or project `docs/working.md` for design mistakes already made in this codebase

Degrade gracefully: if none of these exist, acknowledge it and proceed with heuristic judgment. Say "no design artifacts found — these observations are directional."

### 3. Aesthetic direction

Do not default to the AI mean (Inter, purple gradients, centered hero, card grid, white background). Pick a conceptual direction that serves the product intent, then make every visual choice serve it.

**Avoid the "advanced UI" consensus.** AI design tools have a recognizable default aesthetic: dark backgrounds with glass morphism, bold oversized typography, dramatic spacing, high-contrast accent colors. This reads as sophisticated on first impression, and an AI agent left to its own defaults will reliably produce it. Do not mistake this for good design. This aesthetic is a local maximum — it reliably sacrifices information density, task clarity, platform native feel, and accessibility for visual impact. A recording app dressed in this style looks like a tech demo, not a tool you'd trust with your data.

**Use physical product analogy to anchor direction.** Rather than chasing abstract adjectives ("modern," "clean," "elegant"), ground the interface in a familiar physical product whose functional priorities match yours. Examples:

- A voice recording app → a physical voice recorder: clear transport controls, stable visual feedback, no ambiguity about whether it's recording, every button labeled with its exact function
- A code editor → a workbench: tools within reach, the work surface dominates, chrome is minimal, information density is high
- A reading app → a book: the content is the interface, typography does all the work, navigation is quiet
- A dashboard → a cockpit: critical indicators are always visible, secondary data is one action away, anomalies are visually distinct from normal state

The physical analogy forces every design decision to answer a concrete question: "would the physical version of this product hide this button behind a menu?" "Would a physical recorder make you guess whether it's recording?" The analogy prevents the model from drifting toward visual trends that look good in a portfolio but fail in use.

The direction still has two parts:

**What to amplify** (pick one): information density, editorial calm, playful irreverence, technical credibility, premium restraint, spatial drama, utilitarian clarity

**What to prohibit** (state explicitly): e.g. "no decorative illustrations," "no cards — use rows and dividers," "no gray-on-gray text," "no rounded corners above 4px," "never center-align body text," "no glass morphism if this is a tool — glass communicates lifestyle, not reliability"

This is not about being creative. It's about preventing regression to the mean. The model's default instinct is consensus design. The direction is the reason not to take the default.

### 4. Implementation contract

Only needed when you are writing code. Before generating UI code, write a lightweight contract covering:

- **States**: default, loading, empty, error, disabled, long text, dark mode, reduced motion
- **Component sources**: design system components or new ones? If new, why?
- **Copy**: button labels, error messages, empty states — exact text, not placeholders
- **Platform behaviors**: safe area (iOS), back handling (Android), focus order (Web), Dynamic Type / font scale
- **Edge cases**: minimum content, maximum content, no network, no permission
- **Acceptance screenshots**: which screens must be captured after implementation

This contract is the yardstick you'll use in QA. If you skip it, QA has nothing to measure against.

### 5. Evidence-based QA

After implementation (or when reviewing existing UI), produce real evidence:

1. Capture screenshots (iOS simulator, Android emulator, Web browser) at the target viewport
2. Run deterministic checks first: contrast ratio, touch target size, heading order, ARIA labels, overflow, truncated text, Dynamic Type scaling
3. Then apply model judgment, comparing each screen to the design intent and contract:

**Information hierarchy.** Shrink the screenshot to 50%. What draws the eye first? Is it the primary interactive element (text field, main action, core content), or is it metadata, chrome, or decoration? The visual anchor must be what the user needs to act on. Secondary information — timestamps, voiceprint indicators, word counts, settings — must not compete with the primary task surface. If the user cannot identify the main action in under one second, the hierarchy is wrong.

**Empty states.** An empty text field, an empty list, an empty canvas — these are not blank space. They are the product's first impression. An empty state must answer three questions: what is this, why is it empty, and what should the user do next. An empty recording screen that shows only a waveform without explaining how to start recording has failed. An empty text field without a placeholder that teaches what to type has failed. Empty states that are beautiful but silent are worse than plain ones that teach.

**Task coherence.** Does every visual choice serve the design intent, or are there elements that look good but don't help the user? Decorative elements that survive QA but don't advance the task are the residue of AI consensus design — remove them.

**Platform native feel.** Does this screen feel like it belongs on its platform, or does it feel like a WebView pretending to be native? On iOS, does it respect the navigation bar, swipe-back, and Dynamic Type? On Android, does it respect the back gesture, system bars, and density? On Web, does it handle keyboard navigation, focus order, and responsive breakpoints correctly?

Tools that can be used: `xcrun simctl screenshot`, `adb shell screencap`, Playwright, axe-core, Lighthouse, Accessibility Inspector, manual visual inspection.

If tools are unavailable, do manual visual inspection and label the output accordingly.

### 6. Capture one lesson

After the task, write down one reusable design judgment learned or reinforced. Format:

> **What**: [specific pattern or rule]
> **Why it matters**: [the consequence of getting it wrong]
> **When to use**: [trigger condition]

Example: "Empty states that say 'No items yet' without explaining what an item is or how to create one leave the user stuck. Always include a creation path."

Accumulate these in `docs/working.md` or `rules/design/anti_examples.md`.

## Platform routing

When the target platform is known, apply these platform-specific lenses. The skill file itself does not contain full platform rulebooks — those are loaded from your training data or workspace reference files on demand.

**iOS**: Dynamic Type, safe area insets, navigation bar vs toolbar, swipe-back gesture, SF Symbols, HIG spacing, VoiceOver rotor, 44pt minimum touch target, reduced motion

**Android**: Material 3 components, predictive back gesture, system bars, TalkBack, density independence (dp), edge-to-edge, notification channels, adaptive icons

**Web**: responsive breakpoints, keyboard navigation, ARIA landmarks, focus trap, skip links, prefers-reduced-motion, prefers-color-scheme, semantic HTML heading order, performance (CLS, LCP)

If the platform is unknown, ask. If the user insists on proceeding anyway, note in output: "Platform not specified — platform-specific checks skipped."

## Quality gates

Before presenting a design judgment as complete, verify:

- **Artifact gate**: is there at least one real screenshot or running artifact behind this judgment? If not, label output "directional guidance only."
- **Platform gate**: did platform-specific concerns make it into the judgment? If not, label "platform checks skipped."
- **Specificity gate**: does every finding name a specific element, state a specific problem, and suggest a specific fix? Replace "the layout could be improved" with "the three CTAs in the hero section compete for attention — keep one primary, demote the other two to text links."

## What this skill is not

- A visual design tool or Figma replacement
- A "make it prettier" prompt modifier
- A universal design style guide (taste is contextual to product, audience, and platform)
- An accessibility compliance certification (automated checks catch ~30% of issues; manual testing with real assistive technology is still required for WCAG conformance)

## References

This skill draws on and acknowledges:
- Anthropic's [Design Plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/design) — the original six-skill decomposition (critique, design-system, handoff, ux-copy, accessibility, research-synthesis)
- Anthropic's [Frontend Design Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design) — aesthetic direction via conceptual commitment
- [Evaluation-first methodology](https://yage.ai/share/cursor-agent-harness-evaluation-first-20260501.html) — defining success criteria before building
- [Thin Harness, Fat Skills](https://yage.ai/share/thin-harness-fat-skills-20260414.html) — keeping runtime minimal, loading domain knowledge on demand
