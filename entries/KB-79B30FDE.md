---
id: KB-79B30FDE
subject: "organizationReturns checks the view permission live on each request: removing the grant refuses an already-issued token at once, and re-granting answers a token minted while the grant was gone"
plane: experiential
question: Does a role-grant change apply to organizationReturns before the member signs in again?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.organizationReturns
  - coordinate: PUT /api/organizations
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T16:01:02.862Z
    by: session:62b7421e
    who: Lenajava1
---
On Return 3.1005.0-pr-28-62f9 a holder whose only grant of xapi:my_organization:return:view was an organization-level role was answered by organizationReturns with token T1. After PUT /api/organizations removed that role, the very next call with T1 was Forbidden (no delay). A token T2 minted while the role was absent carried no permission claim and was Forbidden; after the role was put back, T2 (still without the claim) and T1 were both answered immediately. The JWT permission claim is therefore not what gates the query; the server resolves the caller's roles for the queried organization on each call. A test expecting "new grant ignored until re-sign-in" on the server will fail; only the client-side token claims lag.
