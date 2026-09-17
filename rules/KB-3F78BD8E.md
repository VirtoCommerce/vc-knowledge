---
id: KB-3F78BD8E
subject: BL-SR-026 A layout key that was never saved (`null`) is not a failure — registry defaults render with editing enabled
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:31.927Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-026: A layout key that was never saved (`null`) is not a failure — registry defaults render with editing enabled `[P1-data]`

- **Rule:** `null` from the layout query (a never-saved key, BL-SR-015) is a normal, expected state, not an error condition — the surface renders the registry's default arrangement and leaves editing enabled, with no error alert.
- **Verify:** Load a layout-driven surface for a (user, scope, storeId) combination that was never saved → registry defaults render, Edit is enabled, no error/alert appears.
- **Violation signal:** A never-saved key disables Edit, shows an error/alert, or is otherwise treated the same as a genuine read failure.
- **Agents:** qa-frontend-expert
- **Source:** module composable distinguishing a `null` result from a fetch failure. Live-confirmed: a never-saved rep's surface loaded registry defaults with Edit enabled and no alert.
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04 re-audit; source + live CONFIRM; docs N/A). **Split note:** the companion failure-handling behavior (a genuine read failure disables Edit; a genuine save failure keeps the draft and edit mode) remains drafted — its axes are source-only this run; see `bl-proposals`.
