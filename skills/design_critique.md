# Design Critique

Structured design feedback across multiple dimensions. Load this skill when the user asks for a design review, critique, or evaluation of an existing screen — or when the root design_skill routes a critique request to this sub-skill.

Loaded on demand from the root `design_skill.md`. Not intended to be loaded standalone.

## When to use

- User shares a screenshot, Figma link, or live URL and asks for feedback
- Root skill determines that a structured critique is needed before or after implementation
- Reviewing a design at any stage: exploration, refinement, final polish

## What is needed

- The design: screenshot, URL, Figma link, or detailed description
- Context: what platform (iOS/Android/Web), what task the user needs to complete
- Stage: exploration, refinement, or final polish (feedback scope changes by stage)

## Critique framework

Follow this sequence. Each dimension builds on the previous one.

### 1. First impression (2 seconds)

What draws the eye first? Is it the primary task element, or chrome and decoration? Is the screen's purpose immediately readable? The first-glance focal point must match what matters most for the user's task. If it doesn't, the hierarchy is wrong regardless of how polished the individual elements look.

### 2. Usability

Can the user accomplish their goal? Is the navigation intuitive? Are interactive elements obvious? Are there unnecessary steps? Every screen should have exactly one primary action; competing actions should be visually demoted.

### 3. Visual hierarchy

Shrink the screen to 50%. Is there a clear reading order? Whitespace should guide, not decorate. Typography should create explicit layers: the heading says what this is, the body explains, the metadata sits quietly. If everything is emphasized, nothing is.

### 4. Platform consistency

Does this screen follow the platform's interaction model? iOS screens should navigate with push/pop and respect safe areas. Android screens should handle the back gesture and system bars. Web screens should manage responsive breakpoints and focus order. This dimension checks for platform violations that feel like a WebView pretending to be native.

### 5. Accessibility

Minimum checks: color contrast ratios (4.5:1 for normal text, 3:1 for large), touch target sizes (44pt on iOS, 48dp on Android), keyboard navigation (Web), visible focus indicators, text readability at the platform's default font size.

## How to give feedback

- Be specific: "The CTA competes with the navigation bar" not "the layout is confusing"
- Explain why: connect feedback to design principles or user tasks
- Suggest alternatives: identify the problem and propose a fix
- Acknowledge what works: feedback should include positive observations
- Match the stage: early exploration gets directional feedback, final polish gets specific

## Output format

```markdown
## Design Critique: [Screen name]

### Overall impression
[1-2 sentences — what works, what's the biggest gap]

### Usability
| Finding | Severity | Recommendation |
|---------|----------|----------------|
| [Specific issue] | 🔴 Critical / 🟡 Moderate / 🟢 Minor | [Specific fix] |

### Visual hierarchy
- **First focal point**: [element] — [correct or not?]
- **Reading flow**: [how does the eye move through the layout?]
- **Emphasis**: [are the right things emphasized?]

### Platform consistency
| Element | Issue | Recommendation |
|---------|-------|----------------|
| [Pattern] | [Platform violation] | [Fix] |

### What works well
- [Positive observation 1]
- [Positive observation 2]

### Priority recommendations
1. **[Most impactful]** — [why and how]
2. **[Second]** — [why and how]
```
