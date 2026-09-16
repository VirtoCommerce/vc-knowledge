---
id: KB-EF3CB7FB
subject: Login on behalf is authorized on the storefront not in Admin
plane: experiential
question: How do I get a second storefront identity without a second password, and why did Login on behalf fail?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /connect/token
  - coordinate: GET /account/impersonate/:userId
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T07:46:32.673Z
    by: session:6341e00a
---

The Admin account blade's 'Login on behalf' button does not sign anyone in. It opens the STOREFRONT at /account/impersonate/<securityAccountId> in a new tab, and that page POSTs /connect/token with grant_type=impersonate, scope=offline_access and user_id, carrying the STOREFRONT session's own bearer token. Two consequences that cost a run time if unknown. First, the storefront must already be signed in: with an anonymous storefront session the route is a non-public one, so the guard bounces to /sign-in?returnUrl= and the button appears to do nothing. Second, the authorization is the signed-in storefront user's, not the administrator's who clicked - being a platform Administrator in the Admin SPA grants nothing here. Observed on a Customer account explicitly set up as an impersonation operator: after signing that account into the storefront, the same button produced HTTP 403 from /connect/token with an empty body, and the storefront routed to /403. So this is a usable route to a second storefront identity with no second password, but only when the storefront principal itself holds the impersonation grant, and a 403 there tells you it does not - do not read the button's presence in Admin as evidence that it will work.
