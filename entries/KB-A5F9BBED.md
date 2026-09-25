---
id: KB-A5F9BBED
subject: Contact.isLockedInOrganization is resolved from the CALLER's JWT organization claim, so a platform-admin token reads false for a genuinely locked member.
plane: experiential
question: Why does isLockedInOrganization come back false when I query the organization roster with an admin token?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: graphql-xapi
anchors:
  - coordinate: Contact.isLockedInOrganization
  - coordinate: Query.organization.contacts
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-19T14:07:42.209Z
    by: session:local_3b
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:17:49.620Z
    by: session:memimpor
    who: Lenajava1
    note: "Also seen: with a membership locked in a sibling organization, isLockedInOrganization read false for a caller active in another organization; Organization.isLockedForCurrentUser read true for the locked one."
---
Measured on vcst-qa 2026-09-19. Querying organization(id,userId){contacts{items{isLockedInOrganization}}} for org 96f109a7-9010-4691-b6a1-bef25cca3d04 returns isLockedInOrganization=false for fixture "Blocked User" when called with a platform-admin token from /connect/token (no storeId, no organization_id, so no org claim), and true for the same contact when called with an org-scoped storefront token (grant_type=password with storeId=B2B-store and organization_id=<org>). REST ground truth for that membership is isLocked=true, so the admin-token answer is simply wrong rather than empty. The query's own userId argument is NOT what drives it: passing a third party's account id in userId while holding the org-scoped token still returned true. It fails silently — false, not null and not an error — so a caller without an org claim cannot distinguish "not locked" from "cannot tell". Use an org-scoped member token whenever asserting on this field, or read POST /api/customer/organization-memberships/search with organizationId instead.
