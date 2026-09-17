---
id: KB-25F7AAB1
subject: BL-SR-028 Stat cards park/restore by drag or keyboard; widgets hide via a dismiss control and restore only from a hidden-items tray
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:32.390Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-028: Stat cards park/restore by drag or keyboard; widgets hide via a dismiss control and restore only from a hidden-items tray `[P2-ux]`

- **Rule:** A stat card has no per-card dismiss control — it moves between visible and parked by dragging into/out of a parked zone, or by the equivalent keyboard gesture; a mouse-driven park drops at the drop position, a keyboard park appends to the end of the target zone. A widget hides via an explicit dismiss control and can be restored only by choosing it from a hidden-items tray — dragging a hidden widget back in is not a valid restore path. A parked/hidden item is absent from the rendered surface outside edit mode and appears in a distinct "hidden" grouping only in edit mode; the state persists across a reload.
- **Verify:** Confirm a stat card renders no dismiss control; drag a stat card into and out of the parked zone — both work, and the drop lands at the drop position, not appended; a widget's dismiss control hides it, and it can only be restored from the hidden-items tray; outside edit mode a parked/hidden item is absent from the page; in edit mode it appears under a "hidden" grouping; the parked/hidden state survives a reload.
- **Violation signal:** A stat card exposes a dismiss control; a hidden widget can be restored by dragging; a parked/hidden item remains rendered outside edit mode, or its state resets on reload.
- **Agents:** qa-frontend-expert
- **Source:** module layout-region component (park-only toggle for stat cards) and hidden-items tray component (button-only widget restore). Live-confirmed: no dismiss control on a stat card; drag in/out of the parked zone both worked, landing at drop position (vs. keyboard append); widget dismiss→tray→restore confirmed; a parked card is absent from the page outside edit mode and appears under a "hidden" grouping in edit mode, including across a persisted hidden state.
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04 re-audit; source + live CONFIRM; docs N/A).
