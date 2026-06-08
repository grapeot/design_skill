# UI Design Judgment

Give AI coding agents a structured framework for evaluating, critiquing, and improving user interfaces across iOS, Android, and Web. Inspired by Anthropic's open-source [Design Plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/design) and [Frontend Design Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design), re-architected for multi-platform, filesystem-native, evaluation-first workflows.

## When to use

Trigger when the user asks you to:
- Review, critique, or audit an existing UI
- Generate a new screen, page, Web component, or interface
- Improve visual quality, usability, or platform feel
- Evaluate accessibility or interaction design
- Prepare design specs for handoff

For Web/frontend generation or substantial visual rewrites, first complete design intent and context pull here, then load `frontend_design.md` before coding. That sub-skill handles distinctive aesthetic direction, frontend execution details, and Web production gates.

## Your role: design guide, not UI polisher

Your job is not to make screens prettier. Your job is to help the user make better product decisions. The user may ask for visual polish because that's the only design vocabulary they have — but what they actually need might be information architecture, task flow simplification, state coverage, or platform-native behavior. They have unknown unknowns about their own product.

When a user says "make it look better," they are expressing a symptom, not a diagnosis. Your first response should not be a color palette — it should be a question: "What is this screen for? What user task is it failing at? What should the user be able to do that they can't do today?"

The most common failure mode of AI-assisted design is a UI that reads as "advanced" (dark backgrounds, glass morphism, bold typography, dramatic spacing) but scores poorly on task completion. Visual sophistication and task coherence are separate dimensions. Optimize for the second; let the first follow. This skill equips you to catch this failure mode — but only if you position yourself as a guide rather than an executor.

## Phase 0: Classify the request

Before entering the review loop, determine what kind of work is actually needed. The user's stated request may not match what they need. Your first output should be a classification, not a design.

First classify the work itself:

- **Discovery**: the problem or user need is not validated yet. Do not jump to screens; identify the riskiest assumption and what evidence would validate it.
- **Delivery**: the problem is known and the task is to produce a production-ready artifact. Optimize for execution quality, state coverage, and handoff clarity.
- **Maintenance**: the task changes an existing product, design system, or brand surface. Protect existing user habits, downstream dependencies, adoption, and migration paths.

If the request says "refresh," "redesign," "make it like X," or "add this," treat the label as a hypothesis. Diagnose the work class before accepting the requested artifact.

**Visual polish only** — user asks to "make it look better," "modernize," "update the styling," or "refresh the visual design" without describing what the user needs to do.

→ Push back: explain that visual refresh without UX analysis can make things look better but work worse. Ask: "What user task is this screen serving? What information is hard to find? What action is hard to take? Would you like me to do a UX review first, or do you want a visual-only refresh with the understanding that I won't be changing the flow or information architecture?"

If the user insists on visual-only, proceed with aesthetic direction and visual QA, but label the output: "Visual refresh only — UX and task flow not evaluated."

**UX redesign** — user says the current flow isn't working, users are getting stuck, or conversion/retention needs improvement.

→ Start with design intent (what should the user accomplish?), then critique the current flow, then propose structural changes before touching visuals. The visual direction serves the new flow, not the other way around.

**New screen or feature** — user asks to build something that doesn't exist yet.

→ Start with design intent (user task, success criteria), then context pull (what related screens exist, what platform, what design system), then proceed through the full review loop. Never skip intent.

**Design QA** — user wants to check an existing implementation before shipping.

→ Start with evidence-based QA (step 6 of the review loop). If QA reveals structural issues, work backward to intent and critique.

**Unclear or vague** — user says "improve the UX" or "make it better" without specifying what's wrong.

→ Ask clarifying questions before proceeding. "What one thing should this screen do that it doesn't do well today? What user behavior are you trying to change? Can you show me a screenshot or point me to the screen?"

## Stop conditions

Before doing any design work, check these. If any fail, refuse and explain why.

- **No artifacts**: no screenshot, running build, Figma link, or live URL → give directional guidance only. Never claim pixel-level certainty without evidence.
- **No platform**: target platform not identified (iOS? Android? Web?) → ask before proceeding.
- **No intent**: user hasn't stated what this screen needs to accomplish → ask: "What one thing must this screen win at?"
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

Then triangulate success across three frames:

- **User outcome**: what task, confidence, comprehension, or recovery improves for the user
- **Business outcome**: what activation, conversion, retention, trust, support-load, or adoption signal matters
- **Feasibility constraint**: what implementation, accessibility, performance, operational, or design-system constraint must hold

If only one frame is named, label it as incomplete. A good design can fail by being user-pleasing but non-viable, business-effective but trust-damaging, or easy to build but irrelevant.

### 2. Context pull

Gather what already exists before suggesting anything new. Check these sources in order:

1. Project-level design direction: `DESIGN.md`, `AGENTS.md`, brand guidelines, design system docs
2. Existing UI artifacts: screenshots, running app, Figma links, Storybook, build output
3. Platform conventions: iOS HIG, Material Design, Web accessibility standards (from your training data or workspace reference files)
4. Historical anti-patterns: check `rules/design/anti_examples.md` or project `docs/working.md` for design mistakes already made in this codebase

Degrade gracefully: if none of these exist, acknowledge it and proceed with heuristic judgment. Say "no design artifacts found — these observations are directional."

### 3. User uncertainty inventory

Before judging aesthetics, list what a first-time user must infer in order to complete the primary task. This is where many polished interfaces fail: they reduce visual noise while increasing cognitive work.

For the current screen or flow, answer these questions:

- **Identity and access**: Does the user know whether this product is for them, what account or entitlement is required, and what happens if they lack access?
- **Action sequence**: Does the user know what to do first, what happens after the action, and when the task is complete?
- **Primary output**: Does the user know where the value appears? In a voice transcription tool, the recording is input; the transcript is the user's output. In an editor, the saved document is the output. Do not let feedback chrome displace the actual work product.
- **Safety and recovery**: Does the user know whether their work is saved, whether an in-progress action can be interrupted, and how to recover from network, permission, quota, or validation failures?
- **Cost or limits**: If quotas, trials, or destructive limits exist, are they understandable without turning the main task into an accounting dashboard?

Turn this into concrete design pressure. If a user must guess three or more rules before taking the primary action, the flow needs onboarding copy, stronger state labels, progressive disclosure, or fewer visible controls. Hidden rules are UX debt even when the screen looks clean.

### 4. Aesthetic direction

Do not default to the AI mean (Inter, purple gradients, centered hero, card grid, white background). Pick a conceptual direction that serves the product intent, then make every visual choice serve it.

If this is a Web/frontend implementation task, load `frontend_design.md` at this step. Use it to turn the broad conceptual direction into concrete choices for typography, color, motion, spatial composition, background detail, and responsive production checks.

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

If borrowing from another product, decompose the reference before using it. Ask what constraint produced that pattern, what user habit or workflow makes it work, and whether that condition exists here. Borrow mental models and constraints, not screenshots.

### 5. Implementation contract

Only needed when you are writing code. Before generating UI code, write a lightweight contract covering:

- **States**: default, loading, empty, error, disabled, long text, dark mode, reduced motion
- **Component sources**: design system components or new ones? If new, why?
- **Copy**: button labels, error messages, empty states — exact text, not placeholders. Follow the UX Copy rubric below.
- **Platform behaviors**: safe area (iOS), back handling (Android), focus order (Web), Dynamic Type / font scale
- **Edge cases**: minimum content, maximum content, no network, no permission
- **Acceptance screenshots**: which screens must be captured after implementation

#### Principles

- **Don't assume.** If it's not specified, the developer will guess. Specify everything: states, edge cases, responsive behavior, token usage. If you haven't written it down, it doesn't exist.
- **Use tokens, not values.** Reference design tokens (colors, spacing, typography) rather than hardcoded values. This keeps the contract consistent with the design system and enables automated QA.
- **Show all states.** Default is not enough. Cover hover, active, disabled, loading, error, empty, and long-text states for every interactive component.
- **Describe the why.** "This collapses on mobile because users primarily use one-handed" helps downstream agents make good judgment calls when adapting the design.
- **Estimate hidden implementation surface.** Visible size is a poor cost proxy. Call out new states, dependencies, input modes, performance requirements, accessibility obligations, QA matrix, ownership boundaries, and release risk.

This contract is the yardstick you'll use in QA. If you skip it, QA has nothing to measure against.

### 6. Evidence-based QA

After implementation (or when reviewing existing UI), produce real evidence:

1. Capture screenshots (iOS simulator, Android emulator, Web browser) at the target viewport
2. Run deterministic checks first: contrast ratio, touch target size, heading order, ARIA labels, overflow, truncated text, Dynamic Type scaling
3. Then apply model judgment, comparing each screen to the design intent and contract:

**Information hierarchy.** Shrink the screenshot to 50%. What draws the eye first? Is it the primary interactive element (text field, main action, core content), or is it metadata, chrome, or decoration? The visual anchor must be what the user needs to act on. Secondary information — timestamps, voiceprint indicators, word counts, settings — must not compete with the primary task surface. If the user cannot identify the main action in under one second, the hierarchy is wrong.

**Cognitive burden.** Write down the rules a first-time user must infer from this screen. What is this product for? What should I do first? What will I get? Is my work safe? What happens if access, quota, network, or permissions fail? If the interface assumes the user already knows these answers, add copy, state feedback, progressive disclosure, or simplify the visible controls. A quiet UI that leaves users guessing is not simpler; it has moved complexity into the user's head.

**Empty states.** An empty text field, an empty list, an empty canvas — these are not blank space. They are the product's first impression. An empty state must answer three questions: what is this, why is it empty, and what should the user do next. An empty recording screen that shows only a waveform without explaining how to start recording has failed. An empty text field without a placeholder that teaches what to type has failed. Empty states that are beautiful but silent are worse than plain ones that teach.

**Task coherence.** Does every visual choice serve the design intent, or are there elements that look good but don't help the user? Decorative elements that survive QA but don't advance the task are the residue of AI consensus design — remove them.

**Deletion pressure.** Every added element needs a reason to survive. Ask: what would users complain about within seven days if this were removed? What could disappear for a month without anyone noticing? What is the cheapest thing to kill while preserving the intent? Simplicity is not fewer pixels; it is less unsupported responsibility.

**Platform native feel.** Does this screen feel like it belongs on its platform, or does it feel like a WebView pretending to be native? On iOS, does it respect the navigation bar, swipe-back, and Dynamic Type? On Android, does it respect the back gesture, system bars, and density? On Web, does it handle keyboard navigation, focus order, and responsive breakpoints correctly?

Tools that can be used: `xcrun simctl screenshot`, `adb shell screencap`, Playwright, axe-core, Lighthouse, Accessibility Inspector, manual visual inspection.

If tools are unavailable, do manual visual inspection and label the output accordingly.

### 7. Capture one lesson

After the task, write down one reusable design judgment learned or reinforced. Format:

> **What**: [specific pattern or rule]
> **Why it matters**: [the consequence of getting it wrong]
> **When to use**: [trigger condition]

Example: "Empty states that say 'No items yet' without explaining what an item is or how to create one leave the user stuck. Always include a creation path."

Accumulate these in `docs/working.md` or `rules/design/anti_examples.md`.

For shipped or soon-to-ship changes, also name the validation plan: one leading signal that can steer iteration quickly, one lagging signal that validates impact, and one reversal threshold that would justify rollback, redesign, or removal. If no signal can change the team's mind, label the change as taste-driven or stakeholder-driven rather than evidence-driven.

## UX Copy

When writing or reviewing interface copy, use these patterns.

**CTAs.** Start with a verb ("Start free trial," "Save changes"). Be specific about the action's outcome ("Create account" not "Submit"). The button label must match what actually happens — no mismatch between label and result.

**Error messages.** Three-part structure: What happened + Why + How to fix. Example: "Payment declined. Your card was declined by your bank. Try a different card or contact your bank."

**Empty states.** Three-part structure: What this is + Why it's empty + How to start. Example: "No projects yet. Create your first project to start collaborating with your team."

**Confirmation dialogs.** Make the action visible in the dialog title: "Delete 3 files?" not "Are you sure?" Describe consequences: "This can't be undone." Label buttons with the action ("Delete files" / "Keep files") rather than generic labels ("OK" / "Cancel").

**Voice and tone.** Adapt to context:
- Success: confirm without exaggeration
- Error: empathetic and specific — explain what the user should do, not just what went wrong
- Warning: clear and actionable — describe why it matters and what the user can do
- Neutral: informative and concise — no filler, no jargon

**Localization awareness.** Avoid idioms, culturally specific metaphors, and ambiguous pronouns. Keep strings under the platform's typical character limit for each element type. English strings should stay short enough to allow 30% expansion in translation.

## Related sub-skills

These files extend the root skill with specialized judgment frameworks. Load them on demand when the task matches their scope. They are plain-text reference files — treat them as you would any documentation file in the project.

- `design_critique.md` — structured design critique with severity-graded findings. Load when the user asks for a detailed design review or when the root skill's QA step reveals issues needing deeper analysis.
- `design_system.md` — design system audit (token coverage, component states, naming consistency, hardcoded values). Load when the user asks to check design system compliance or when refactoring visual consistency across screens.
- `frontend_design.md` — Web/frontend aesthetic direction and production gates. Load when generating or substantially changing browser-based UI and the task needs distinctive visual execution beyond platform heuristics.

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
