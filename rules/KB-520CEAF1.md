---
id: KB-520CEAF1
subject: BL-SR-015 Configurable layout is keyed by rep + surface + optional store; a never-saved key resolves null; per-user isolation
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:29.412Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-015: Configurable layout is keyed by rep + surface + optional store; a never-saved key resolves null; per-user isolation `[P1-data]`

- **Rule:** `salesRepLayout` / `saveSalesRepLayout` address a document keyed on the calling user's id, the `scope` argument (a per-surface identifier, e.g. `"dashboard"` / `"customerProfile"`), and an optional `storeId` — three distinct `storeId` values (omitted, a nonexistent store, a real store) address three distinct documents. A (user, scope, storeId) combination that was never saved resolves `salesRepLayout` to `null`, never an error. A rep's saved layout is never visible to a different rep querying the same scope.
- **Verify:** Query `salesRepLayout` with a fresh `scope` → `null` (SR-GQL-099/103/116). Save under one `storeId`, query under a different `storeId`/omitted → `null`, not the saved doc (SR-GQL-104). Two different authenticated reps querying the same `scope` each resolve only their own document, never the other's (SR-GQL-102).
- **Violation signal:** A scope/storeId combination that was never saved returns anything but `null`; a rep's saved layout is visible to a different rep querying the same scope; `storeId` omitted and a real value resolve the same document.
- **Agents:** qa-backend-expert
- **Source:** `vc-module-sales-rep` `LayoutService.GetLayoutAsync`/`BuildNameParts` (keys the customer-preference lookup on `[PreferenceName, scope, storeId?]` under the resolved `userId`); `SalesRepLayoutQuery.Map` (`UserId = context.GetCurrentUserId()`).
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04; source + live CONFIRM; docs N/A — pre-GA module, undocumented).
