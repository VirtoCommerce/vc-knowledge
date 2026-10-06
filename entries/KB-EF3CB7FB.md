---
id: KB-EF3CB7FB
subject: Login on behalf is authorized on the storefront not in Admin
plane: experiential
question: How do I get a second storefront identity without a second password, and why did Login on behalf fail?
questions:
  - text: Why does clicking the button to shop as this customer in the back office do nothing?
  - text: Whose authorization is checked when an admin uses Login on behalf for a storefront customer?
  - text: Which grant type and token does the impersonate storefront route send to the token endpoint?
  - text: Which permission must the signed-in storefront user hold for impersonation to succeed rather than 403?
  - text: Can I get a second storefront identity for testing without knowing that user's password?
concepts:
  - id: impersonation
  - id: permission
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: GET /account/impersonate/:userId
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-13T07:46:32.673Z
    by: session:6341e00a
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:17:23.645Z
    by: session:memimpor
    who: Lenajava1
    note: The permission that gates this is the platform key platform:security:loginOnBehalf (Admin roles picker label 'Login on behalf of a customer'), which older storefront wording calls CanImpersonate.
---

The Admin account blade's 'Login on behalf' button does not sign anyone in. It opens the STOREFRONT at /account/impersonate/<securityAccountId> in a new tab, and that page POSTs /connect/token with grant_type=impersonate, scope=offline_access and user_id, carrying the STOREFRONT session's own bearer token. Two consequences that cost a run time if unknown. First, the storefront must already be signed in: with an anonymous storefront session the route is a non-public one, so the guard bounces to /sign-in?returnUrl= and the button appears to do nothing. Second, the authorization is the signed-in storefront user's, not the administrator's who clicked - being a platform Administrator in the Admin SPA grants nothing here. Observed on a Customer account explicitly set up as an impersonation operator: after signing that account into the storefront, the same button produced HTTP 403 from /connect/token with an empty body, and the storefront routed to /403. So this is a usable route to a second storefront identity with no second password, but only when the storefront principal itself holds the impersonation grant, and a 403 there tells you it does not - do not read the button's presence in Admin as evidence that it will work.
