---
id: KB-B152EF7F
subject: BL-B2B-005 Member role determines feature visibility
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:07.553Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-B2B-005: Member role determines feature visibility `[P1-data]`

- **Rule:** Organization features visible on the storefront depend on the member's role. Org Admins see: member management, quotes, order approval, lists. Buyers see: order placement (within limits), lists, own orders. Members without purchasing role see: catalog browsing only. Feature visibility is controlled by both role permissions and the store's feature flags (`quotesEnabled`, etc.).
- **Verify:** Sign in as Org Admin → see "Members", "Quotes", "Approval" menu items. Sign in as Buyer → see "Orders", "Lists" but NOT "Members." Sign in as view-only member → no cart, no checkout access.
- **Violation signal:** Buyer sees member management; non-purchasing member can add to cart; features visible when feature flag is OFF; role change not reflected until re-login.
- **Data path (VCST-5028):** Permission-gated features read `pageContext.user.permissions`, which MUST be populated from the **active `OrganizationMembership.Roles`** after an org-switch. The global `ApplicationUser.Roles` is no longer the source of truth for org-scoped visibility. (BUG-A: the org-scoped JWT was correct but the `me`/GetPageContext projection returned `permissions:[]`, hiding maintainer actions — see BL-B2B-007.)
- **Org-level roles (VCST-5239):** beyond per-member `OrganizationMembership.Roles`, an org can carry **org-level roles** (`Organization.Roles`) inherited by **all** its members. Effective perms = the **deduped union** of org-level-role ∪ membership-role ∪ global roles (storefront `getContactRoles` unions org+global). Removing a member's own membership-role override MUST preserve the org-inherited perms (verified — no strip-the-base regression).
- **Agents:** qa-frontend-expert (storefront nav), qa-backend-expert (org roles API)
