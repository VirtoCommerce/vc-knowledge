---
id: KB-A2DAE212
subject: BL-SR-014 Embedded Sales Rep Admin app gates on customer-member + platform-security permissions, not on `sales-rep:access`
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: POST /api/sales-rep/search
  - coordinate: PUT /api/sales-rep
evidence:
  - method: observation
    at: 2026-09-17T15:21:29.152Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-014: Embedded Sales Rep Admin app gates on customer-member + platform-security permissions, not on `sales-rep:access` `[P1-data]`

- **Rule:** The embedded Sales Rep Admin app (`api/sales-rep`) is gated by the **customer module's member permissions + platform security permissions**, NOT by `sales-rep:access` (which only defines a storefront rep) and NOT merely by the module being installed. The exact matrix (`[Authorize]` attributes; **multiple attributes = AND — all required**):
  - **Read** (`search`, `roles`, `dictionaries`, `GET {id}`) → **`customer:read`**.
  - **Create** → **`customer:create` AND `platform:security:create`**.
  - **Update** → **`customer:update` AND `platform:security:update`**.
  - **Delete** → **`customer:delete` AND `platform:security:delete`**.
  - **Account-only ops** (`{id}/block`, `{id}/unblock`, `{id}/password`) → **`platform:security:update` only** (NOT `customer:update`) — a distinct mutate class from entity CRUD.
  - An `isAdministrator` account bypasses all checks.
- **Verify:** a back-office Manager (`isAdministrator=false`) **without `customer:read`** gets the menu entry hidden and `POST /api/sales-rep/search` → 302 → AccessDenied; a Manager with **`customer:read` only** opens the app and lists reps (search → 200) but Add/Delete/Save are hidden and `POST`/`PUT /api/sales-rep` → **403**; block/unblock/reset-password succeed only with `platform:security:update` (independent of `customer:update`).
- **Violation signal:** a Manager lacking `customer:read` reaches the rep list; a `customer:read`-only Manager creates/edits/deletes a rep (UI action present or create/update API 2xx); or block/unblock/set-password succeeds without `platform:security:update`.
- **Agents:** qa-backend-expert.
- **Source:** `vc-module-sales-rep` `SalesRepController.cs` (`dev`, `api/sales-rep`) — per-endpoint `[Authorize]` map (`CustomerModule…Permissions.Read/Create/Update/Delete` + `Platform…Permissions.SecurityCreate/Update/Delete`); `useSalesRepPermissions/index.ts` (frontend UI gate — CRUD subset, no account-ops class); `ModuleConstants.cs` (`sales-rep:access` = rep definition only). VCST-5293.
- **Note:** the read-only edit blade also needs store/org read for its dropdowns (a separate `store:*`/org-read dependency surfaced live) — a UI-completeness dependency, not part of the RBAC gate.
- **Promoted:** 2026-07-24 (TLC-2026-07-24-1906; BL-AUDIT-2026-07-24 — ex-BL-SREP-003). Evidence bar: **applicable-axes** — live (SR-ADM-023 **5-account API matrix** — no-access / read-only / account-ops / member-only / full-non-admin — every cell matched: `customer:read` read gate; account-ops = `platform:security:update` only (204); create/update/delete = customer:* AND platform:security:* (403 when either half is missing); FULL non-admin clears every gate — real `[Authorize]` chain, not admin bypass) + source (controller `[Authorize]` map) CONFIRM; **docs N/A** (module pre-GA / undocumented). Sibling BL-SR-011 (hub org-membership carve-out) promoted on source authority (live-verify pending deploy).

---
