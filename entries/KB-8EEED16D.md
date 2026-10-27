---
id: KB-8EEED16D
subject: After a platform account lock, a sales-rep access token issued before the lock is refused on /graphql/sales-rep with extensions.code Unauthorized (not UserLocked)
plane: experiential
question: What does /graphql/sales-rep return when a rep's account is locked after their token was issued?
questions:
  - text: Does locking a user invalidate their existing access token for the sales-rep schema?
  - text: Which error code does salesRepCustomerOrders return for a token minted before the account lock?
  - text: Is the query-time account-state gate reported as UserLocked or Unauthorized?
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /graphql/sales-rep
  - coordinate: Query.salesRepCustomerOrders
  - coordinate: POST /api/platform/security/users/{id}/lock
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; store=B2B-store
    at: 2026-10-20T10:36:03.350Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; module:VirtoCommerce.TaskManagement=3.1005.0; store=B2B-store
    at: 2026-10-27T21:46:17.119Z
    by: session:e75fe8a4
    bodyHash: befa06baee7f
    who: Aleksandra-Mitricheva
    note: "Re-observed 2026-10-27 on SalesRep 3.1016.0-pr-26-92d4: prelock totalCount 19; after POST /lock the SAME token got HTTP 200, data.salesRepCustomerOrders null, errors[0].extensions.code Unauthorized ('Anonymous access denied or access token has expired or is invalid.'); new password grant 400 invalid_grant; after unlock a fresh token returned 19 again. The lock helper's immediate read-back again saw lockoutEnd unchanged (read-after-write lag) although the lock had applied."
---
2026-10-20, 1 run: password-grant token minted (storeId param) for an unlocked rep; salesRepCustomerOrders(first:1) returned totalCount 19. Account then locked via POST /api/platform/security/users/{id}/lock (lockoutEnd 9999-12-31, status Locked). Re-sending the identical query with the SAME pre-lock token returned HTTP 200, data.salesRepCustomerOrders null, errors[0].message 'Anonymous access denied or access token has expired or is invalid.', extensions.code Unauthorized — twice, not UserLocked. A new password grant was refused 400 invalid_grant with code user_is_locked_out. After POST /unlock a fresh token returned data again. Note the admin GET of the user right after POST /lock still read lockoutEnd null for a moment (read-after-write lag), while the token was already refused.
