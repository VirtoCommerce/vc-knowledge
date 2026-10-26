---
id: KB-8D5B9AC9
subject: productPickupLocations / cartPickupLocations ignore a keyword that contains an apostrophe (or is only '&') and return every location, while Query.pickupLocations returns 0 for the same keyword
plane: experiential
question: How do the pickup-location keyword searches treat special characters such as an apostrophe or ampersand?
status: active
appliesTo:
  - axis: feature
    value: bopis
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.productPickupLocations
  - coordinate: Query.cartPickupLocations
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T00:25:27.100Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T00:43:47.702Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Re-run 2026-10-15 as admin token: productPickupLocations \"O'Neil\" -> 102, pickupLocations -> 0. Root cause and other syntax-character cases are in a separate entry about the keyword phrase parser."
  - method: observation
    deployment: vcptcore_regression
    build: 5609fc0c36e7302c
    conditions: platform=3.1076.1; theme=2.59.0; module:VirtoCommerce.XPickup=3.1008.0; module:VirtoCommerce.Shipping=3.1009.0; module:VirtoCommerce.Search=3.1010.0; role=anonymous
    at: 2026-10-21T13:06:10.072Z
    by: session:624d35a1
    bodyHash: fbaca263d04e
    defect: suspected
    who: kutasinaelena
    note: "2026-10-21 anonymous xAPI, product with 102 eligible locations: productPickupLocations and cartPickupLocations both 102 for \"O'Brien's\", \"&\", \"'\", \"O'Neil\"; pickupLocations 0. \"Toronto\" 12/10/10, \"zzqqxx\" 0, quoted \"\\\"O'Brien's\\\"\" 0. Docs grammar (Search Query Syntax) allows ' and & in SimpleString, so marked as suspected defect."
  - method: observation
    deployment: vcst_qa
    build: c65e39a5daec993f
    conditions: module:VirtoCommerce.XPickup=3.1008.0; module:VirtoCommerce.Shipping=3.1009.0; module:VirtoCommerce.Search=3.1010.0; module:VirtoCommerce.XCart=3.1042.0; role=anonymous
    at: 2026-10-21T13:06:11.500Z
    by: session:624d35a1
    bodyHash: fbaca263d04e
    who: kutasinaelena
    note: "Same on vcst_qa 2026-10-21: scoped queries return all 115 for ' / & keywords, while store-level pickupLocations finds 3 locations containing \"&\" and 4 containing \"'\" - the scoped searches drop the keyword even when literal matches exist."
  - method: observation
    deployment: vcst_qa
    build: 04c60d6136307709
    conditions: role=B2B organization buyer; store=B2B-store
    at: 2026-10-26T22:45:08.764Z
    by: session:c2534117
    bodyHash: fbaca263d04e
    who: kutasinaelena
    contradicts: true
    note: "2026-10-27 vcst_qa after the vc-module-search search-phrase grammar fix (VCST-6477), I signed in as a B2B buyer and used the storefront /cart pickup dialog. cartPickupLocations (cart with one product eligible for 115 locations) returned: empty keyword 115; \"O'Neil\" 0 three runs out of three, and the dialog showed \"Pickup points not found.\"; \"Arts & Crafts\" 0; \"Children's\" 2 (both Children's Museum locations); \"Stop & Shop\" 1 three runs out of three; \"Barclays'\" 1; \"West\" 6. No errors[] in any response. Apostrophe and ampersand keywords are no longer dropped on this stand."
  - method: observation
    deployment: vcptcore_regression
    build: 82521b7bb31c0bcd
    conditions: module:VirtoCommerce.Search=3.1010.0; store=B2B-store; culture=en-US; role=anonymous
    at: 2026-10-26T22:47:30.174Z
    by: session:c2534117
    bodyHash: fbaca263d04e
    defect: VCST-6477
    who: kutasinaelena
    note: "2026-10-27: still true on Search 3.1010.0 — productPickupLocations \"O'Neil\" and \"&\" return 102 of 102, 3 of 3 runs. The fix (vc-module-search PR #159, ' and & added to SimpleString) is verified on vcst_qa running the unreleased build 3.1012.0-pr-159-eec5, where the same keywords return 0 (see KB-9F8787D7). A dispute filed from that PR build does not make this entry wrong for 3.1010.0; set fixedIn once PR #159 ships in a release."
---
2026-10-15 vcptcore_regression (signed-in buyer, product with 102 eligible pickup locations). productPickupLocations(keyword): "zzqq" -> 0, "Neil Sons" -> 0, "Neil & Sons" -> 0, but "O'Neil" -> 102, "O'Neil & Sons" -> 102, "'" -> 102, "&" -> 102 (totalCount = all). The storefront cart 'Select address' pickup dialog sends the same keyword to cartPickupLocations and shows all 17 pages for "O'Neil & Sons" instead of the no-results message. Anonymous Query.pickupLocations(storeId, keyword:"'" or "&") returns 0 on both vcptcore_regression and vcptcore_stable, so only the product/cart-scoped searches drop such a keyword.
