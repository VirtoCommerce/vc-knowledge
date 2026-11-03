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
---
On XCart 3.1045.0-pr-153-ec03 (VCST-6185 fix) I signed in on the storefront as two personal customers and called Mutation.mergeCart through the storefront /graphql. (1) User B merging user A's named B2B-store cart (secondCartId = A's cart id, userId = B, cartName = B's named cart, deleteAfterMerge omitted or false) got HTTP 200 with data.mergeCart = null and errors[0] {message "Access denied.", extensions.code "Forbidden"}, 3 of 3 times. A's cart still existed with the same item and quantity, and B's target cart was unchanged. (2) An anonymous call (no Authorization header, or the storefront's anonymous-session token) with A's cart id got errors[0] code "Unauthorized", message "Anonymous access denied or access token has expired or is invalid."; A's cart was intact. (3) B merging B's OWN cart from another store (Electronics) into a B2B-store target got "Forbidden", and the source was left in place. (4) B merging B's own same-store named cart succeeded: the items arrived in the target and the source was deleted (deleteAfterMerge default). (5) A secondCartId matching no cart (a random GUID) returned the unchanged target cart with no error. Before the fix, the same call from (1) succeeded and deleted the victim cart.
