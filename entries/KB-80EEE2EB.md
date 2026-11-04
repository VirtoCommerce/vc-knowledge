---
id: KB-80EEE2EB
subject: On a header Ship to selection a personal user's storefront sends updateContact, GetFullCart (cart not loaded yet), AddOrUpdateCartShipment, GetMe, then GetCurrentUserAddresses(ids:[selected]); the label changes to the new address in the same round trip
plane: experiential
question: Which GraphQL requests does the storefront send when a personal user picks another address in the header Ship to popover, and does the label update at once?
status: active
appliesTo:
  - axis: frontend
    value: 2.62.0-pr-2424
  - axis: surface
    value: storefront-ui
  - axis: user
    value: personal
anchors:
  - coordinate: Mutation.updateContact
  - coordinate: Mutation.addOrUpdateCartShipment
  - coordinate: Query.me
  - coordinate: Query.currentCustomerAddresses
  - coordinate: /account/addresses
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: theme=2.62.0-pr-2424-cdad-cdade315; browser=chrome; store=B2B-store
    at: 2026-11-04T13:25:30.428Z
    by: session:3f1092dc
    who: Lenajava1
---
Signed in as a personal user (no organization) with 8 address-book entries, theme 2.62.0-pr-2424 with /graphql proxied to the QA platform, in Chrome. Picking another address in the header popover (on /account/addresses and /cart, with and without a cart item, from page 1, page 2 and a search-filtered list) always produced this request sequence: updateContact (command.selectedAddressId = picked id), GetFullCart (only when the cart was not loaded on the page), AddOrUpdateCartShipment (deliveryAddress.id = picked id), GetMe, GetCurrentUserAddresses (variables first:1, ids:[picked id]). The Ship to label showed the picked address after every one of 7 selections without a reload, and the cart page Shipping address matched. Cross-check through the API: 16 consecutive updateContact calls each followed at once by me.contact.selectedAddressId returned the new id every time (updateContact 143-1303 ms, me 143-341 ms), so the platform shows no read-after-write lag for this field. A report of the label staying on the old address for 3-8 s on a Firefox lane was not reproduced.
