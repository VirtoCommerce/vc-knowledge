---
id: KB-E2C5A66A
subject: BL-UI-003 No state-induced layout shift
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:18.845Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-UI-003: No state-induced layout shift `[P2-ux]`

- **Rule:** Hover, focus, validation-message insertion, badge/counter updates, and skeleton → content swap MUST NOT move adjacent elements. A border that appears on hover must use `outline` (which does not affect layout) OR reserve its space with a transparent border in the default state. A counter widening from 1-digit to 3-digit must not push siblings.
- **Verify:** Record `getBoundingClientRect()` of a neighbor sibling (`rectSnapshotSnippet(selector)`). Trigger the state change. Re-record. Compare with `compareRectSnapshots(before, after)` — `topDelta` and `leftDelta` must be 0.
- **Violation signal:** Neighbor moves on hover. Form below a field jumps when validation error inserts. Cart-icon badge change shifts navbar items. Skeleton dimensions ≠ resolved content → snap on load.
- **Agents:** ui-ux-expert (components), qa-frontend-expert (cart/checkout/forms)
- **Suite coverage:** NONE — was `048b` LAYOUT-SHIFT-001..003 (product-card hover, cart-badge update, validation error insertion)
 (suite removed 2026-07-25; audit manually via `/qa-design`)
- **Promoted:** 2026-05-14 (from `ui-ux-expert.md` UI-invariants draft).
