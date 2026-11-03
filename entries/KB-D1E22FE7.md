---
id: KB-D1E22FE7
subject: mergeCart lets an administrator merge another customer's same-store cart, but still refuses a cart of another store with Forbidden
plane: experiential
question: Can an administrator merge another customer's cart with mergeCart secondCartId, and is a cart of another store refused for an administrator too?
concepts:
  - id: cart
status: active
appliesTo:
  - axis: role
    value: administrator
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.mergeCart
  - coordinate: InputMergeCartType.secondCartId
evidence:
  - method: observation
    deployment: vcst_qa
    build: 8cc253c1304af90a
    conditions: module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store; role=platform administrator
    at: 2026-11-03T15:17:57.733Z
    by: session:3107e574
    who: Lenajava1
    note: "distinct: The existing entry covers non-owner customers and anonymous callers; this one is the administrator exemption from the owner check and the administrator's refusal at the store boundary, observed with an administrator token."
  - method: observation
    deployment: vcst_qa
    build: 8cc253c1304af90a
    conditions: module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store; role=platform administrator
    at: 2026-11-03T15:28:40.462Z
    by: session:3107e574
    bodyHash: e4f0d47379f1
    who: Lenajava1
    note: "GraphQL runner CRL-GQL-133 (admin merges USER2's same-store cart: allowed, both products, source deleted) and CRL-GQL-134 (admin, cart of another store: Forbidden, source and target unchanged)."
---
On XCart 3.1045.0-pr-153-ec03 I ran the GraphQL runner with the platform administrator token (role ADMIN) against Mutation.mergeCart on /graphql. (1) A personal customer's named B2B-store cart as secondCartId, userId = the administrator's own id, cartName = the administrator's named target cart, deleteAfterMerge true: no error, the customer's item and the target's own item were both in the returned cart, and the customer's cart no longer resolved afterwards. (2) The same customer's named cart in the Electronics store (its storeId differs from the command's B2B-store) as secondCartId: HTTP 200, data.mergeCart null, errors[0].extensions.code Forbidden; the Electronics cart still existed and the administrator's target was unchanged. So the administrator passes the owner check but not the store check.
