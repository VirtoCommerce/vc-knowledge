---
id: KB-EB4DECDC
subject: "mergeCart refuses a secondCartId the caller may not access: Forbidden for a signed-in non-owner, Unauthorized for anonymous, Forbidden for a cart of another store; nothing is merged or deleted"
plane: experiential
question: Can a signed-in caller merge another customer's cart into their own with mergeCart secondCartId, and what error comes back?
questions:
  - text: What does mergeCart return when secondCartId belongs to another user?
  - text: Does mergeCart check the store of the second cart?
  - text: Can an anonymous caller call mergeCart with a registered user's cart id?
  - text: What happens when mergeCart secondCartId matches no cart?
  - text: Is the source cart deleted when mergeCart is refused?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.mergeCart
  - coordinate: InputMergeCartType.secondCartId
evidence:
  - method: observation
    deployment: vcst_qa
    build: 8cc253c1304af90a
    conditions: module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store; role=personal customer
    at: 2026-11-03T14:47:46.523Z
    by: session:3107e574
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 8cc253c1304af90a
    conditions: module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store; role=personal customer
    at: 2026-11-03T14:59:29.431Z
    by: session:6875a995
    bodyHash: 244746d5630a
    who: Lenajava1
    note: "Second round: anonymous 3/3 Unauthorized and signed-in 3/3 Forbidden on fresh victim carts; also a signed-in user merging an anonymous guest cart (isAnonymous=true) by secondCartId is allowed and the items arrive."
  - method: observation
    deployment: vcst_qa
    build: 8cc253c1304af90a
    conditions: module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store; role=B2B org member
    at: 2026-11-03T15:17:43.500Z
    by: session:3107e574
    bodyHash: 244746d5630a
    who: Lenajava1
    note: "Via the GraphQL runner as ORG_USER (B2B org member) merging USER2's named and default carts: Forbidden, victim intact; USER2's own Electronics cart into a B2B-store target: Forbidden, source and target unchanged; a removed cart's id as secondCartId: no error, target unchanged; own named cart merge: allowed."
  - method: observation
    deployment: vcst_qa
    build: 8cc253c1304af90a
    conditions: platform=3.1093.0-pr-3154-4607; theme=2.62.0-pr-2600-a4f2-a4f29534; module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store; role=personal customer
    at: 2026-11-03T15:28:40.365Z
    by: session:3107e574
    bodyHash: 244746d5630a
    who: Lenajava1
    note: "Storefront /graphql from the page: anonymous mergeCart (no Authorization, cookies omitted) on a signed-in customer's named cart -> HTTP 200, mergeCart null, Unauthorized; victim intact, anonymous target never created. Runner CRL-GQL-130/131: signed-in non-owner Forbidden, own cart of another store Forbidden. A signed-in customer merging an isAnonymous=true guest cart by secondCartId is allowed: both products in the target, guest cart deleted."
  - method: observation
    deployment: vcst_qa
    build: 8cc253c1304af90a
    conditions: module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store; role=personal customer
    at: 2026-11-03T15:35:53.500Z
    by: session:3107e574
    bodyHash: 244746d5630a
    who: Lenajava1
    note: "Round 2 re-walk on a fresh browser context: anonymous mergeCart on a signed-in customer's named cart gave Unauthorized again, victim intact. A guest cart (isAnonymous=true, random guest id) merged by a signed-in customer by secondCartId: both items arrived and the guest cart was deleted."
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: theme=2.62.0-pr-2596-e749-e749b47f; module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store; role=personal customer
    at: 2026-11-03T15:49:10.925Z
    by: session:3107e574
    bodyHash: 244746d5630a
    who: Lenajava1
    note: "Round 3 on theme 2.62.0-pr-2596: anonymous mergeCart is still Unauthorized and the victim is intact; a guest isAnonymous cart merged by a signed-in customer is still allowed."
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: platform=3.1093.0-pr-3154-4607; theme=2.62.0-pr-2596-e749; module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; store=B2B-store
    at: 2026-11-03T16:20:30.712Z
    by: session:3107e574
    bodyHash: 244746d5630a
    who: Lenajava1
    note: "Regression REG-2026-11-03-1717 via the GraphQL runner: CRL-GQL-130..134, 074, 075 all PASS on the same build (non-owner Forbidden, anonymous Unauthorized, other-store cart Forbidden, own-cart merge allowed)."
---
On XCart 3.1045.0-pr-153-ec03 (VCST-6185 fix) I signed in on the storefront as two personal customers and called Mutation.mergeCart through the storefront /graphql. (1) User B merging user A's named B2B-store cart (secondCartId = A's cart id, userId = B, cartName = B's named cart, deleteAfterMerge omitted or false) got HTTP 200 with data.mergeCart = null and errors[0] {message "Access denied.", extensions.code "Forbidden"}, 3 of 3 times. A's cart still existed with the same item and quantity, and B's target cart was unchanged. (2) An anonymous call (no Authorization header, or the storefront's anonymous-session token) with A's cart id got errors[0] code "Unauthorized", message "Anonymous access denied or access token has expired or is invalid."; A's cart was intact. (3) B merging B's OWN cart from another store (Electronics) into a B2B-store target got "Forbidden", and the source was left in place. (4) B merging B's own same-store named cart succeeded: the items arrived in the target and the source was deleted (deleteAfterMerge default). (5) A secondCartId matching no cart (a random GUID) returned the unchanged target cart with no error. Before the fix, the same call from (1) succeeded and deleted the victim cart.
