---
id: KB-3E84BFA8
subject: After a personal user deletes their selected Ship to address, the header shows "Select address" but the cart/checkout keeps the deleted address as shipping and billing
plane: experiential
question: What does the storefront header Ship to and the checkout shipping address show after the contact's selected address is deleted from the address book?
questions:
  - text: Does checkout keep a shipping address that was deleted from the address book?
  - text: Does the header Ship to selector fall back to the cart shipment address after the selected address is deleted?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: /account/addresses
  - coordinate: Query.currentCustomerAddresses
  - coordinate: Mutation.deleteMemberAddresses
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: theme=2.62.0-pr-2424-cdad-cdade315; module:VirtoCommerce.ProfileExperienceApiModule=3.1023.0; account=personal
    at: 2026-11-04T12:48:45.559Z
    by: session:3f1092dc
    who: Lenajava1
---
As a personal storefront user (no organization) with 8 addresses, the selected address (page 2, not a favorite) was deleted on /account/addresses. The header Ship to selector then read "Select address" in the same tab, after a reload, on catalog pages and after in-app navigation to /cart. The selected-address lookup (currentCustomerAddresses with ids = the selected id) returned 0 items with no errors, and Contact.selectedAddressId kept pointing at the deleted id. The cart shipment's deliveryAddress has its own id, different from the address-book id, so the header's cart-shipment fallback did not match it. The /cart checkout section still showed the deleted address as the Shipping address, with billing "Same as shipping". So the header and checkout disagree, and an order could be placed to an address no longer in the book. Not compared with the build before theme 2.62.0-pr-2424.
