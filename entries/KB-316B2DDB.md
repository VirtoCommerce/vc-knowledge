---
id: KB-316B2DDB
subject: one invalid storeId answers Unauthorized on Query.product and NULL_REFERENCE on Query.products
plane: experiential
question: What does the storefront xAPI do when a product query carries an argument that names something that does not exist — an unknown store, currency, culture, product id or filter field?
status: active
appliesTo:
  - axis: surface
    value: storefront-xapi
anchors:
  - coordinate: Query.product
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-18T18:25:17.220Z
    by: session:kbs4demo
---
There is no single rule, and the trap is that the failure shape is chosen per ARGUMENT rather than per query — so what you learn from one invalid argument tells you nothing about the next. Five shapes, all answering HTTP 200, all observed anonymously against B2B-store on vcst_qa (4,550 products) and each reproduced in a second pass ~25 minutes later.

1. AN UNKNOWN storeId IS REPORTED AS AN AUTH FAILURE ON ONE FIELD AND A NULL REFERENCE ON THE OTHER. `product(id:<real>, storeId:"NO-SUCH-STORE")` returns `errors[0].extensions.code: "Unauthorized"` with the message "Anonymous access denied or access token has expired or is invalid." — while the very same anonymous request against `storeId:"B2B-store"` returns the product. So the message is misattributed: nothing about the credentials changed, and a reader debugging it will go and look at tokens. `products(storeId:"NO-SUCH-STORE")` on the same deployment, same second, answers `NULL_REFERENCE` instead. One bad argument, two codes, neither of which says "that store does not exist".

2. An unknown `currencyCode` errors on BOTH fields, and consistently: `INVALID_OPERATION`, with `data.product` / `data.products` null. This is the one argument whose behaviour you can generalise across the pair.

3. An unknown product `id` is a SILENT null: `product(id:"NO-SUCH-PRODUCT-ID", storeId:"B2B-store")` returns `{"data":{"product":null}}` with no `errors` array at all. Indistinguishable, in the response, from a product the caller is not allowed to see.

4. An unknown `cultureName` is IGNORED: `cultureName:"zz-ZZ"` returns the full 4,550-product page, same first item as `en-US`. No error, no fallback marker.

5. An unknown filter field, and an unknown product id in `productIds`, both return `totalCount: 0` with an empty `items` — the same answer a correct query over an empty result set gives.

What it means for a test. Three of these five (3, 4, 5) fail with no `errors` array, so a case that asserts only "no errors" passes on a typo. Assert the RETURNED VALUES: a non-zero `totalCount` where one is expected, the culture actually reflected in the payload, the id you asked for present in the result. And when an xAPI query answers "Anonymous access denied" on a deployment where anonymous browsing plainly works, check the storeId before you check the token — @kb(KB-D9B90536) records the sibling trap on the same operation, where an unknown sort field is ignored and the page still looks sorted.
