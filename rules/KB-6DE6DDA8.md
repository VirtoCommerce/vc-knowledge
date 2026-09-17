---
id: KB-6DE6DDA8
subject: BL-SR-024 Configurable-layout changes persist only on explicit Save, are scoped to the rep's account, and survive reload and re-authentication
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:31.473Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-024: Configurable-layout changes persist only on explicit Save, are scoped to the rep's account, and survive reload and re-authentication `[P1-data]`

- **Rule:** No drag, hide, or reorder action is persisted on its own — each is a draft-only change until the rep explicitly saves, and one Save issues exactly one full-document replace (BL-SR-016), never more. The persisted arrangement is scoped to the rep's account rather than a device or browser session: reloading, signing out and back in, or opening an independent browser session all resolve the same saved arrangement. Cancel and Reset discard the in-progress draft without issuing any save.
- **Verify:** Perform a drag/hide/reorder without saving, then reload → the pre-change arrangement still renders; Save once → exactly one save call is issued; a reload and a fresh sign-in both then show the saved arrangement; Cancel and Reset both complete with zero save calls.
- **Violation signal:** An unsaved change survives a reload; a drag/hide/reorder issues a save call by itself; Cancel or Reset issues a save call; the arrangement differs after sign-out/sign-in.
- **Agents:** qa-frontend-expert, qa-backend-expert
- **Source:** module composable governing the edit draft (`save()` as the sole mutation call site; reorder/hide actions touch only the draft) — consistent with BL-SR-015 (account-scoped key) and BL-SR-016 (full-document replace). Live-confirmed: save→reload returned the arrangement exactly; a second save fully replaced the first save's state with no leftovers; Cancel and Reset each issued zero network requests; the arrangement survived a full sign-out/sign-in.
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04 re-audit; source + live CONFIRM; docs N/A — pre-GA module, undocumented).
