# Working Log

## Changelog

--- 2026-06-07

- Renamed sub-skills per consensus: design_skill.md (root), design_critique.md, design_system.md
- Added UX Copy rubric (inline, standalone section)
- Added Implementation Contract principles (don't assume, tokens not values, show all states, describe the why)
- Added Related sub-skills section referencing design_critique.md and design_system.md
- Injected real-world UX insights: physical product analogy, advanced-UI consensus trap, information hierarchy check, empty states as first citizens, visual polish stop condition
- Merged via PR #1

--- 2026-06-07 (later)

- Added Phase 0 request classification: agent as design guide, not UI polisher. Five categories with specific responses.
- Merged via PR #3

--- 2026-06-08

- Updated installation guidance for the multi-file skill family: install the repo or complete `skills/` directory, expose only the root skill, and do not symlink `design_skill.md` alone.
- Added `frontend_design.md` as an on-demand sub-skill for distinctive Web/frontend aesthetic direction, adapted from Anthropic's Frontend Design Plugin spirit while preserving the root skill's evaluation-first flow.
- Updated README, AGENTS, RFC, and root skill routing to reflect the new sub-skill.

--- 2026-06-08 (later)

- Folded selected product-designer practitioner judgments into the public skill family without exposing internal persona machinery: work-class framing, success criteria triangulation, reference discipline, deletion pressure, hidden implementation surface, validation signals, critique layering, and design-system governance.

--- 2026-10-07

- Absorbed Anthropic's latest design guidance (all sources fetched and verified upstream 2026-10-07). Rebased onto the then-current master, which already carried `frontend_design.md` and the practitioner-judgment pass, so this entry only covers what was still missing:
  - Root `design_skill.md` §4: added subject-matter grounding (name the concrete subject, audience, and primary job before choosing a direction).
  - Root §5 (Implementation contract): added responsive behavior and motion to the contract list; clarified the handoff spec-sheet structure for specs consumed by another engineer.
  - Root §6 (Evidence-based QA): added "separate observation from interpretation."
  - Root References + intro: added Agent Skills `frontend-design`, the Frontend Aesthetics cookbook, and WCAG 2.1 AA; tagged per-item source attribution.
  - `frontend_design.md`: added Anthropic's current-defaults calibration catalog, the type/line-length/structural-device rules, and a "plan, then defend it against the brief" + "spend boldness in one place" section.
  - `design_critique.md`: expanded the accessibility dimension with the WCAG 2.1 AA criterion list, common issues, and the automated→manual testing approach.
  - `design_system.md`: added the tokens/components/patterns model, system principles, a motion token row, and component document/extend templates.
  - `README.md`: listed the newer Anthropic sources and the calibration/WCAG ideas.

## Lessons Learned

- **Distinguish first-party Anthropic content from third-party retellings.** Three official sources matter and differ: the Cowork `design` plugin (six workflow skills, Figma/MCP-oriented), the `claude-code` frontend-design plugin, and the newer `anthropics/skills` repo. The `anthropics/skills` `frontend-design` SKILL.md was the freshest (2026-09) and is byte-identical to the `claude-code` copy; the Cowork design plugin was last touched 2026-09.
- **Check origin before starting.** A parallel change had already landed `frontend_design.md` and the practitioner-judgment pass on master. Starting from a stale local checkout would have duplicated the frontend aesthetic work and conflicted. Rebase first, then only add what is still missing.
- **Our skill is stronger in scope and weaker in specifics.** We already cover multi-platform, an evaluation-first flow, stop conditions, and an uncertainty inventory that Anthropic's skills do not. Anthropic's advantage is specific, checkable calibration (named default traits, WCAG criterion numbers, handoff and system templates). Absorption should move specifics in without diluting our structure.
- **The "AI default" warning needs updating, not just repeating.** The original "advanced UI consensus" framing (dark glass, bold type) is still true but dated; Anthropic now catalogs warmer, more current clusters (cream + terracotta serif, SaaS card kit, template chrome). Concrete catalogs age better than adjectives.
- **Root length is a real constraint.** The root skill is ~275 lines; the repo already softened its earlier ~200-line target to "stay compact." Further specialized absorption should go into a sub-skill rather than the root.
- **Scope discipline on research skills.** We deliberately did not absorb Anthropic's `user-research` / `research-synthesis` skills; our artifact is UI design judgment, not a product-research program. The one transferable idea (separate observations from interpretations) was folded into the QA step.
