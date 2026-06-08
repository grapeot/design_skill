# Test Strategy

## What we test

The `design_skill` is a Markdown skill file consumed by AI coding agents. Traditional automated testing (unit/integration) does not apply directly. Instead, we test through:

### Acceptance Criteria (per-task verification)

After each real-world use of the skill, verify:

1. **Did the agent produce a judgment, or just prose?** The output must contain at least one specific finding (element + problem + recommendation), not just general commentary.

2. **Did the agent respect evidence gates?** If no screenshot/build/artifact was available, the output must contain "directional guidance only" or equivalent acknowledgment.

3. **Did the agent stop when it should?** If the task involved a destructive action (delete, payment, privacy), the output must show elevated confirmation and copy review.

4. **Did the agent route to the correct platform?** If the task specified iOS/Android/Web, platform-specific concerns (e.g., Dynamic Type for iOS, back behavior for Android, responsive for Web) must appear in the judgment.

5. **Did the skill stay short?** The root skill file must remain under 150 lines. If growth is needed, extract a sub-skill rather than inflating the root.

### Design review scenarios (manual QA)

Run these scenarios with an AI agent equipped with the skill and verify the output qualitatively:

| # | Scenario | Expected behavior |
|---|---|---|
| 1 | New iOS settings page, simulator screenshot available | Agent produces structured critique with iOS-specific concerns (Dynamic Type, safe area, VoiceOver) |
| 2 | Existing Web dashboard, only a description provided | Agent gives directional guidance, labeled as such; does not claim pixel-level certainty |
| 3 | Destructive action flow (delete account) | Agent elevates confirmation copy, risk explanation, and button semantics |
| 4 | No platform specified, no artifacts | Agent asks for platform and evidence before proceeding |
| 5 | Responsive page, desktop + mobile screenshots | Agent checks both viewports, notes overflow and touch target differences |
| 6 | UI generation request without design intent | Agent asks "what must this win at?" before generating |

### Regression check

Before publishing a new version of the skill file:

1. Re-run scenario 4 (no platform, no artifacts) — it should still refuse to guess
2. Re-read the skill file — it should still be under 150 lines
3. Verify all referenced tools (Xcode, Playwright, etc.) are mentioned as optional with degradation paths

## What we don't test (yet)

- Automated CI testing — the skill is a Markdown file; there's no build step
- Visual regression — this would require a reference UI suite that doesn't exist yet
- Platform-specific tool availability — depends on the user's environment
