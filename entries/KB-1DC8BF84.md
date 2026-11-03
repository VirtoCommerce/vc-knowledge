---
id: KB-1DC8BF84
subject: "UCP anonymous-to-authenticated cart merge: replaying the saved anonymous buyer_id under an authenticated token merges, and a second replay is an idempotent no-op"
plane: experiential
question: What happens when the saved anonymous buyer_id is replayed through UCP /ucp/mcp update_cart under an authenticated token, and then replayed a second time?
status: active
appliesTo:
  - axis: buyer
    value: signed-in
  - axis: surface
    value: ucp-mcp
anchors:
  - coordinate: Mutation.mergeCart
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: platform=3.1093.0-pr-3154-4607; module:VirtoCommerce.XCart=3.1045.0-pr-153-ec03; module:VirtoCommerce.Ucp=3.1010.0; store=B2B-store
    at: 2026-11-03T16:20:30.829Z
    by: session:3107e574
    who: Lenajava1
---
On vcst_qa, through the storefront-host UCP MCP endpoint /ucp/mcp with a signed-in buyer token (B2B personal customer), replaying the saved anonymous buyer_id and cart_id in update_cart merges the anonymous cart into the buyer's cart (UCP delegates to XCart mergeCart), and a second replay of the same call is an idempotent no-op. Case UCPA-014 PASSED in regression REG-2026-11-03-1717 on XCart 3.1045.0-pr-153-ec03, i.e. the VCST-6185 mergeCart authorization still lets an isAnonymous guest cart be merged.
