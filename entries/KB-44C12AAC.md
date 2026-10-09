---
id: KB-44C12AAC
subject: On /account/lists/{id} the line quantity input renders max="0" and is exposed as invalid on an untouched render when the product has no maximum quantity; cart and PDP inputs do not
plane: experiential
question: What min/max does the quantity input on the list details page /account/lists/{id} get, and is it reported invalid, for a product whose maxQuantity is 0 (no limit)?
status: active
appliesTo:
  - axis: page
    value: list-details
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists/{id}
  - coordinate: Query.wishlist
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T11:24:58.526Z
    by: session:df9d131f
    who: Lenajava1
---
2026-10-09, storefront 3.0.0-alpha.2685: list with one in-stock product (xAPI product.minQuantity 1, maxQuantity 0, availableQuantity 1225). Untouched list details page: <input type=number min=1 max=0 value=1 aria-label="Product quantity">; validity.rangeOverflow=true, :invalid matches, Chromium accessibility tree reports spinbutton invalid=true, valuemin=1, valuemax=0, with no visible error text. Typing 5 shows no error and Add to cart works, so the effect is a programmatic invalid state (screen readers announce the field invalid), not a functional block. Same product on /cart renders min=1 max=1225 (invalid=false) and on the PDP min=0 max=1225 (invalid=false). Also reproduced on storefront 2.59.0-pr-2458 (vcptcore_qa), product with maxQuantity 0 / availableQuantity 248: identical min=1 max=0, AX invalid=true.
