# Frontend Design

Create distinctive, production-grade Web interfaces that avoid generic AI-generated aesthetics. Load this skill when the root `design_skill.md` routes a Web/frontend generation task here: pages, components, landing pages, dashboards, prototypes, or application screens implemented in HTML/CSS/JS, React, Vue, Svelte, or similar stacks.

Loaded on demand from the root `design_skill.md`. Not intended to be loaded standalone.

## When to use

- The target platform is Web or browser-based
- The agent is writing or substantially changing frontend code
- The user asks for a page, component, application UI, visual refresh, or distinctive frontend direction
- Existing output looks like generic AI UI: Inter, purple gradients, centered hero, card grid, rounded white panels, decorative blobs

Do not use this as a substitute for UX intent. Start with the root skill's design intent and implementation contract. This sub-skill only controls aesthetic direction and frontend execution quality.

## Concept first

Before coding, commit to one clear conceptual direction. The direction must explain four things:

- **Purpose**: what user task or product promise the interface serves
- **Tone**: the aesthetic world the UI belongs to
- **Constraint**: the technical, accessibility, performance, brand, or content limits that matter
- **Memory hook**: the one visual or interaction idea a user would remember later

Pick a direction with enough force to prevent regression to the mean. Examples: brutally minimal, editorial/magazine, industrial/utilitarian, playful/toy-like, luxury restraint, retro-futuristic, organic/natural, art deco/geometric, brutalist/raw, soft/pastel, dense operator console.

The point is not intensity. Bold maximalism and refined minimalism can both work. The point is commitment: every type, color, motion, spacing, and background decision should belong to the same idea.

## Reference discipline

References are constraint-output pairs, not screenshots to imitate. Before making something "like" another product, name the source product's native constraint, the user habit or workflow it relies on, and the part that transfers to this product. If the source constraint does not exist here, the visual language is probably aspiration rather than design evidence.

Use references to borrow mental models, interaction primitives, density choices, and tradeoff reasoning. Do not borrow a surface style just because it looks mature. AI-generated references are especially weak evidence because they usually lack provenance; treat them as prompts for discrimination, not proof of taste.

## Avoid consensus frontend

Never default to the common AI frontend bundle:

- Inter, Roboto, Arial, or unexamined system fonts
- Purple gradients on white backgrounds
- Centered hero plus card grid as the default page structure
- Generic rounded cards, glass morphism, decorative blobs, and vague SaaS illustrations
- Evenly distributed palettes with no dominant visual position
- Motion sprinkled everywhere without a designed moment

If the existing product has a design system, follow it. Otherwise, make choices that are specific to the product context rather than statistically common across SaaS templates.

## Execution dimensions

### Typography

Choose type with character. Pair a distinctive display face with a readable body face when the product can support it. For utilitarian products, a restrained or native font may be correct, but choose it deliberately and document why. Avoid default font choices unless platform convention is the aesthetic strategy.

### Color and theme

Use CSS variables or project tokens. Commit to a dominant color world with sharp accents, or a deliberate monochrome system with precise contrast. Do not spread attention evenly across many soft colors. Check contrast before claiming the design is production-ready.

### Motion

Use motion as choreography, not decoration. One well-designed page load, reveal sequence, hover transition, or state change is stronger than many unrelated micro-interactions. Respect `prefers-reduced-motion`. Prefer CSS-only motion for simple HTML; use the project's existing motion library when available.

### Spatial composition

Avoid default symmetry unless the concept requires calm formality. Consider asymmetry, overlap, strong grids, diagonal flow, editorial spacing, controlled density, or a dominant work surface. Responsive behavior is part of composition, not an afterthought.

### Background and detail

Create atmosphere only when it serves the concept. Texture, grain, geometric patterns, gradient meshes, shadows, borders, and custom cursors can help, but they must reinforce the product's tone. Remove any decorative layer that survives only because it looks impressive.

## Complexity matching

Match implementation complexity to the aesthetic vision.

Maximalist concepts need enough code to deliver the idea: layered composition, animation timing, depth, and responsive containment. Minimalist concepts need precision: spacing, type scale, alignment, hover/focus states, and contrast. A minimal UI with sloppy spacing is unfinished; a maximalist UI without system is noise.

## Production gates

Before handing off frontend work, verify:

- The implemented screen has a named conceptual direction
- The direction names a product principle or constraint, not just a style label
- Typography, color, motion, composition, and background details all support that direction
- The UI works at mobile and desktop breakpoints
- Focus states, keyboard navigation, semantic HTML, and contrast were checked
- Reduced motion is respected when motion exists
- AI-generated surfaces, components, or copy were treated as provisional until reviewed against product intent, accessibility, brand/context fit, and hidden implementation cost
- There is at least one screenshot or running artifact behind the judgment

If these gates are not met, label the result as a prototype or directional draft, not production-grade frontend.
