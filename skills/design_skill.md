# UI Design Judgment

Give AI coding agents a structured framework for evaluating, critiquing, and improving user interfaces across iOS, Android, and Web. Inspired by Anthropic's open-source [Design Plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/design) and [Frontend Design Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design), re-architected for multi-platform, filesystem-native, evaluation-first workflows.

## When to use

Trigger when the user asks you to:
- Review, critique, or audit an existing UI
- Generate a new screen, page, or interface
- Improve visual quality, usability, or platform feel
- Evaluate accessibility or interaction design
- Prepare design specs for handoff

## Stop conditions

Before doing any design work, check these. If any fail, refuse and explain why.

- **No artifacts**: no screenshot, running build, Figma link, or live URL → give directional guidance only. Never claim pixel-level certainty without evidence.
- **No platform**: target platform not identified (iOS? Android? Web?) → ask before proceeding.
- **No intent**: user hasn't stated what this screen needs to accomplish → ask: "What one thing must this screen win at?"
- **Destructive action**: delete, payment, auth, privacy, medical, financial → elevate confirmation copy review and explain risks before touching UI.

## The review loop

Follow this sequence. Don't skip steps. If a step can't be completed due to missing information, degrade explicitly rather than pretending.

### 1. Design intent

Before looking at any UI, state what success means for this specific task. Exactly one primary goal. Examples:

- "This onboarding must get the user to the first real action in under 30 seconds."
- "This error state must reduce panic so the user reads the recovery instructions."
- "This dashboard must let an operator spot anomalies in one scan."

Not: "make it look modern" or "improve the UX." Those aren't goals — they're symptoms of not having one.

### 2. Context pull

Gather what already exists before suggesting anything new. Check these sources in order:

1. Project-level design direction: `DESIGN.md`, `AGENTS.md`, brand guidelines, design system docs
2. Existing UI artifacts: screenshots, running app, Figma links, Storybook, build output
3. Platform conventions: iOS HIG, Material Design, Web accessibility standards (from your training data or workspace reference files)
4. Historical anti-patterns: check `rules/design/anti_examples.md` or project `docs/working.md` for design mistakes already made in this codebase

Degrade gracefully: if none of these exist, acknowledge it and proceed with heuristic judgment. Say "no design artifacts found — these observations are directional."

### 3. Aesthetic direction

Do not default to the AI mean (Inter, purple gradients, centered hero, card grid, white background). Pick a conceptual direction that serves the product intent, then make every visual choice serve it.

The direction has two parts:

**What to amplify** (pick one): information density, editorial calm, playful irreverence, technical credibility, premium restraint, spatial drama, utilitarian clarity

**What to prohibit** (state explicitly): e.g. "no decorative illustrations," "no cards — use rows and dividers," "no gray-on-gray text," "no rounded corners above 4px," "never center-align body text"

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
   - Does the first-glance focal point match what should matter most?
   - Is the information hierarchy correct at 50% scale?
   - Does every interactive element have a visible state?
   - Does the copy say exactly what it needs to, with no filler?
   - Does this feel native to its platform, or like a WebView pretending?

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
