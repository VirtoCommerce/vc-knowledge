---
id: KB-EDFA4DBF
subject: Cart new-address dialog accepts a digit in Last name; updateMemberAddresses then fails silently and checkout continues
plane: experiential
question: What happens on /cart when a new shipping address has a digit in the last name?
status: active
appliesTo:
  - axis: account
    value: personal
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Mutation.updateMemberAddresses
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: platform=3.1099.0; theme=2.63.0-pr-2602-6507ff26; module:ProfileExperienceApiModule=3.1025.0
    defect: suspected
    at: 2026-11-18T15:48:00.802Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; role=personal customer; delivery=Pickup; payment=Manual
    at: 2026-11-26T02:39:37.884Z
    by: session:779b94fc
    bodyHash: 579b48ea317e
    defect: suspected
    who: kutasinaelena
    note: New BILLING address with Last name 'Wave3' accepted by the dialog; Place order sent UpdateMemberAddresses (errors[0] 'Last Name' contains invalid characters) and CreateOrderFromCart succeeded (order created). Here a toast 'Some of the addresses are not valid' WAS shown and the page stayed on /cart while the order already existed; the next Save in the address dialog navigated to /checkout/completed.
---
As a new personal storefront customer with an empty address book, the /cart 'select a shipping address' dialog accepted Last name 'Subs007' (no inline error) and Create set it as the cart shipment address (addOrUpdateCartShipment succeeded). On Place order the storefront called updateMemberAddresses, which returned HTTP 200 with errors[0] message "'Last Name' contains invalid characters.", extensions code VALIDATION, field addresses[0].lastName, errorCode InvalidCharacters, data null. No message was shown to the buyer, the order was still created and paid, the console logged an unhandled ApolloError, and currentCustomerAddresses stayed empty afterwards.
