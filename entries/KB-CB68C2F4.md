---
id: KB-CB68C2F4
subject: "A Saved-for-later round trip keeps a configurable line's File-section files: same /api/files/{id} urls, still downloadable 200 by the cart owner"
plane: experiential
question: Does moving a configurable line with File-section attachments to Saved for later and back keep the files?
status: active
appliesTo:
  - axis: file-scope
    value: product-configuration
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Mutation.moveToSavedForLater
  - coordinate: Mutation.moveFromSavedForLater
  - coordinate: /cart
  - coordinate: GET /api/files/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    conditions: module:VirtoCommerce.XCart=3.1039; module:VirtoCommerce.FileExperienceApi=3.1006; theme=2.60.0-pr-2485-f6cd22b2; role=customer
    at: 2026-10-13T10:02:33.048Z
    by: session:a3a809eb
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    conditions: module:VirtoCommerce.XCart=3.1041.0; module:VirtoCommerce.FileExperienceApi=3.1008.0; theme=2.60.0-pr-2485-f6cd22b2
    at: 2026-10-13T12:01:19.459Z
    by: session:60eb3dd3
    bodyHash: 06b47aeed0b6
    who: Aleksandra-Mitricheva
    note: "Re-observed 2026-10-13 with 2 files (pdf 611 B, txt 30 B) on XCart 3.1041.0: new lineItemId, same urls, download 200 identical bytes; the Saved-for-later card shows no configuration; file display order varied between renders."
---
2026-10-13 on vcptcore_qa (storefront theme 2.60.0-pr-2485), buyer on B2B-store: a configurable line with 3 files in its File section (pdf 611 B, txt 31 B, txt 2151 B) was moved with Save for later (cart became empty) and back with Move to cart. The line got a new lineItemId (5c98… -> f0a9…), and its configuration still listed the same 3 /api/files/{id} urls; downloading one afterwards returned 200 with identical bytes. The display order of the three files varied between renders (upload order was not kept). The /account/saved-for-later page itself shows no configuration (no Components list) for the line.
