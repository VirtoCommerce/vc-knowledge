---
id: KB-BE7DF8DA
subject: "Customer share switched to AnyoneAnonymous: targets and message cleared, key kept"
plane: experiential
question: What happens to a Customer-scope wishlist share when changeWishlist sets scope AnyoneAnonymous?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.changeWishlist
  - coordinate: Query.sharedWishlist
evidence:
  - method: observation
    deployment: vcptcore
    at: 2026-10-01T13:07:26.990Z
    by: session:680740e1
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore
    at: 2026-10-02T08:44:53.593Z
    by: session:7c4c6f53
    who: Aleksandra-Mitricheva
    note: "2026-10-02 theme 2.59.0-pr-2476-31b5 via storefront Share dialog: Customer(AcmeCorp)->Anyone with link kept sharingSetting.id, targets [] message null; target-org member, another-org member and anonymous all read via sharedWishlist(key) with access Read. Back to Specific customers with a customer picked -> same key, anonymous Unauthorized, non-target org Forbidden."
---
On x-cart pr-141 / cart pr-194: changing a Customer list with targets and a message to scope AnyoneAnonymous keeps sharingSetting.id, sets targets [] and message null; anonymous sharedWishlist(key) then returns the list with access Read; a member of the former target org still gets Access denied from wishlist(listId) and reads only by key. Switching it back to Customer without addSharedWithIds is refused INVALID_OPERATION (empty set) and leaves AnyoneAnonymous in place.
