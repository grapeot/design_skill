# Design System Audit

Audit a UI for design system consistency: token usage, component coverage, naming conventions, and hardcoded value detection. Load this skill when the user asks to check design system compliance, audit component usage, or standardize visual language across screens.

Loaded on demand from the root `design_skill.md`. Not intended to be loaded standalone.

## When to use

- Checking whether a screen or component uses the project's design tokens correctly
- Auditing component variants and state coverage
- Detecting hardcoded values (colors, spacing, typography) that should be tokens
- Reviewing naming consistency across a codebase or design file
- Preparing for a design system migration or cleanup

## What is needed

- The target screen(s): screenshots, code files, or component names
- The design system reference: DESIGN.md, token definitions, or a link to the token spec
- Platform: iOS, Android, or Web (affects token naming conventions and tooling)

## Audit dimensions

### 1. Token coverage

Check whether visual properties reference tokens or use hardcoded values. For Web, check CSS/Tailwind for hardcoded hex `#` values against the token system. For iOS, check for hardcoded Color literals instead of asset catalog references. For Android, check for hardcoded `#` values instead of theme attributes.

| Category | Token | Hardcoded | Issue |
|----------|-------|-----------|-------|
| Colors | 12 defined tokens | 7 hardcoded hex values | Low consistency |
| Spacing | 8 tokens | 23 hardcoded px values | Rules may be broken |
| Typography | 6 tokens | 3 inline font declarations | Minor |

### 2. Naming consistency

Check whether similar elements use the same names across the project. Inconsistent naming (e.g., `btn-primary` in one file and `buttonPrimary` in another) indicates the design system is not being enforced at the code level.

| Issue | Location | Recommended standard |
|-------|----------|---------------------|
| Mixed naming: `primaryBtn` vs `button-primary` | 3 components | Use `button--primary` |
| Shadow token missing from system, hardcoded in 5 places | 5 files | Add `shadow-level-1` token |

### 3. Component state coverage

For each key component, check how many of the expected states are implemented:

| Component | default | hover | active | disabled | loading | error | Score |
|-----------|---------|-------|--------|----------|---------|-------|-------|
| Button | ✅ | ✅ | ⚠️ | ✅ | ❌ | ❌ | 3/5 |
| Input | ✅ | ✅ | ⚠️ | ✅ | ✅ | ❌ | 4/5 |
| Card | ✅ | ❌ | ❌ | N/A | N/A | N/A | 1/3 |

Missing states leave developers guessing. Document the expected state list for each component.

### 4. Pattern compliance

Common patterns should follow the same visual rules across the UI. Check for:

- Are all cards the same border radius and shadow depth?
- Are all CTAs consistently positioned in similar layouts?
- Do similar form layouts use the same spacing and alignment?
- Are error states visually consistent across different features?

## Output format

```markdown
## Design System Audit: [Project / Feature]

### Summary
**Tokens checked:** [X] | **Hardcoded values found:** [X] | **Components audited:** [X]

### Token coverage
| Category | Defined | Hardcoded | Status |
|----------|---------|-----------|--------|
| Colors | [X] | [X] | ✅ / ⚠️ / ❌ |
| Spacing | [X] | [X] | ✅ / ⚠️ / ❌ |
| Typography | [X] | [X] | ✅ / ⚠️ / ❌ |

### Component state gaps
| Component | Missing states | Impact |
|-----------|--------------|--------|
| [Component] | [States] | [User-facing consequence] |

### Priority actions
1. [Most impactful fix]
2. [Second]
3. [Third]
```
