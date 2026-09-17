---
id: KB-96DC8C1D
subject: BL-B2B-010 Self-service company registration grants org-membership roles only, never global roles
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: /sign-up
  - coordinate: GET /api/platform/security/users/{userName}
  - coordinate: /api/customer/organization-memberships/search
evidence:
  - method: observation
    at: 2026-09-17T15:21:08.297Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-B2B-010: Self-service company registration grants org-membership roles only, never global roles `[P1-data]`

- **Rule:** Self-service company registration (storefront `/sign-up`, "Organization" account type) MUST create the `Organization`, the registrant `Contact`, and an `OrganizationMembership` for (newUserId, newOrg) carrying the registrant's org role(s) (e.g. org-maintainer); the registration flow MUST NOT write any platform security role to the global `ApplicationUser.Roles`. This is the registration-time counterpart of BL-B2B-009 (which covers the invite path). (VCST-5028.)
- **Verify:** Register a new company with a fresh `AGENT-TEST-` user. `POST /api/customer/organization-memberships/search {userEmail}` → membership exists with non-empty org role(s) and `isActive=true`. `GET /api/platform/security/users/{userId}` → global `roles[]` empty / contains no platform admin role. The two assertions together prove roles live in the membership record, not on the global account.
- **Violation signal:** After registration the global `ApplicationUser.Roles` is non-empty with platform roles; or no membership/org role is created for the new organization.
- **Agents:** qa-frontend-expert (registration flow), qa-backend-expert (members + organization-memberships + security/users API)
