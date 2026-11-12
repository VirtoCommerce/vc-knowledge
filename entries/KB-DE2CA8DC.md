---
id: KB-DE2CA8DC
subject: "A storefront Block user on /company/members locks only that org membership: a password grant naming the locked org returns 200 with organization_id switched to the user's other org, a refresh_token grant naming it returns 400 user_is_locked_in_organization"
plane: experiential
question: What happens at /connect/token for a multi-org buyer whose membership in one organization was blocked from the storefront members page?
questions:
  - text: Does a password grant fail when organization_id names an org where the user is locked?
  - text: Which grant returns user_is_locked_in_organization?
  - text: Does storefront Block user lock the global account?
status: active
appliesTo:
  - axis: principal
    value: org-maintainer
  - axis: store
    value: b2b-store
  - axis: surface
    value: rest
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /connect/token
  - coordinate: /company/members
  - coordinate: POST /api/customer/organization-memberships/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: theme=2.62.0-alpha.2647; store=B2B-store
    at: 2026-11-12T10:07:26.483Z
    by: session:9c36bcd2
    who: Aleksandra-Mitricheva
---
On vcst_qa, store B2B-store, an org maintainer blocked a member (Approved in org A as Organization maintainer, in org B as Organization employee) via /company/members Actions > Block user > OK. The org A membership became isLocked=true, org B stayed unlocked, and /api/platform/security/users/{id}/locked stayed {locked:false}. POST /connect/token grant_type=password with storeId=B2B-store and organization_id=A returned 200 with organization_id claim = B; grant_type=refresh_token with that refresh token and organization_id=A returned 400 invalid_grant, code user_is_locked_in_organization. A storefront sign-in landed in org B. After Unblock user both grants resolved org A again. Without storeId every grant for this user returned 400 user_cannot_login_in_store.
