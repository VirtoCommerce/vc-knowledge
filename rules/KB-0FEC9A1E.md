---
id: KB-0FEC9A1E
subject: BL-B2B-007 Per-org JWT permission set is org-scoped; pageContext must match it
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: UserType.permissions
evidence:
  - method: observation
    at: 2026-09-17T15:21:07.854Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-B2B-007: Per-org JWT permission set is org-scoped; pageContext must match it `[P0-revenue]`

- **Rule:** A JWT issued for org X MUST carry only the `permission[]` derived from `OrganizationMembership.Roles` for (userId, orgX); permissions from any other org MUST NOT appear. `pageContext.user.permissions` (the `me`/GetPageContext projection) MUST equal the active-org JWT `permission[]`. (VCST-5028.)
- **Org-level role perms in the JWT (VCST-5239):** org-level-role permissions also flow into the org-scoped JWT via `OrganizationIdClaimProvider` (adding an org-level role raised a member's token 11→12 perms). The union (org-level ∪ membership ∪ global) stays strictly org-scoped — no cross-org leak — and `pageContext.user.permissions` still equals the decoded JWT for the active org.
- **Verify:** User is org-maintainer in X, org-employee in Y. Switch to X → decode JWT → maintainer set present, employee-only set absent. Switch to Y → only employee set. For each org, `GetPageContext` → `user.permissions` matches the decoded JWT for that org.
- **Violation signal:** JWT carries another org's permissions; `pageContext.user.permissions` diverges from the JWT (BUG-A condition — pageContext returned `[]` while the JWT held 8 maintainer perms).
- **Agents:** qa-frontend-expert (org switcher, pageContext), qa-backend-expert (token minting, xAPI me resolver)
