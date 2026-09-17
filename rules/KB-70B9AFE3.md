---
id: KB-70B9AFE3
subject: BL-AUTH-005 RBAC 6-permission model
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: POST /api/catalog/products
evidence:
  - method: observation
    at: 2026-09-17T15:21:05.139Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-AUTH-005: RBAC 6-permission model `[P1-data]`

- **Rule:** Every module in Virto Commerce follows the same permission-claim model built from a canonical base set — `access`, `read`, `create`, `update`, `delete` (the 5-permission module template in `ModuleConstants.cs`) — plus `export` on modules that support data export, and further module-specific extensions (e.g. Orders adds `read_prices` / `update_shipments`). The commonly-used "6-permission" shorthand = the base 5 + `export`. Permissions are assigned to roles, and roles are assigned to users. A user without `create` permission on a module must not see the "Create" button in Admin. API calls without the required permission must return 403 Forbidden.
- **Role whitelist is NOT a permission boundary (VCST-5239):** the Customer-module Organization/Membership role **whitelists** populate assignment dropdowns only; they are **not** server-enforced — assigning a non-whitelisted role via xAPI `changeOrganizationContactRole` / REST succeeds (`succeeded:true`). The `access/read/…/403` model governs module *operations*, not which roles may be *assigned*. Do NOT treat a non-whitelisted-role assignment as a 403/security case unless a server boundary is later added.
- **Verify:** Create a role with only `read` on Catalog → assign to user → sign in as that user → "Create" and "Delete" buttons absent in Catalog blade. Attempt `POST /api/catalog/products` → 403. Add `create` permission → button appears.
- **Violation signal:** Buttons visible for unauthorized actions; API returns 200 instead of 403; user can create/delete without permission; `access` permission not required to enter module.
- **Agents:** qa-backend-expert (RBAC API, Admin SPA), qa-testing-expert (permission testing)
- **Source:** Platform docs "Global permissions" (the module permission template is `access/read/create/update/delete`; Orders extends it with `read_prices`) + `vc-platform` `ClaimsPrincipalExtensions.cs:61-71` (`HasGlobalPermission` = reserved-administrator short-circuit, else the permission claim must be held) + `vc-module-customer` `ModuleConstants.cs:14-34` (base 5 plus a module-specific `invite`, and a separate organization-membership permission group) with `OrganizationMembershipController.cs:22,34,80,92,119,129,137` gating each action by permission. Live-confirmed on the environment: the registered permission catalog is dominated by the base-5 verbs across every module group, `export` appears only on the few modules that support data export, and a storefront-scoped token carrying no platform permission claims received 403 from three permission-gated admin endpoints. The Admin-SPA button-visibility half of `Verify` was not re-observed this run (docs + source cover the UI-gating mechanism).
- **Amended:** 2026-08-05 (auto-applied, triangulated — BL-AUDIT-2026-08-05). No Rule change; `Source:` added with docs + source + live anchors.
