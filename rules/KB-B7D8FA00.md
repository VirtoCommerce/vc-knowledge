---
id: KB-B7D8FA00
subject: BL-SR-030 A save already in flight cannot be duplicated by a rapid repeat trigger
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:32.897Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-030: A save already in flight cannot be duplicated by a rapid repeat trigger `[P2-ux]`

- **Rule:** Triggering Save again while a save is already in flight never results in a second full-document-replace call reaching the backend — exactly one save request is issued per user-intended save, regardless of how a repeat trigger is delivered.
- **Verify:** Rapidly trigger Save twice in immediate succession → exactly one save mutation call is observed.
- **Violation signal:** Two save mutation calls are observed for a single rapid repeat trigger.
- **Agents:** qa-frontend-expert
- **Source:** module composable's save guard (client-side only — the backend has no concurrency guard of its own; each save is an independent full replace, per BL-SR-016). Live-confirmed: a rapid double-trigger on the Save control produced exactly one save mutation call; the specific guard mechanism (composable guard vs. a disabled control swallowing the second click) could not be isolated from the UI alone, so this Rule is stated as the observable **outcome**, not the mechanism.
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04 re-audit; source + live CONFIRM of the outcome; docs N/A).
