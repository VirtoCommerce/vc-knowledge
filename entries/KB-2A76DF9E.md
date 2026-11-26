---
id: KB-2A76DF9E
subject: On /cart a signed-in user saves a Skyflow card by ticking the inline form's 'Save card for future payments. It's safe.' checkbox before Place order; the card then appears on /account/saved-credit-cards as '•••• 0015, Expires 12/29'
plane: experiential
question: How does a Skyflow card get saved for a signed-in user, and where does it show up afterwards?
status: active
appliesTo:
  - axis: payment
    value: skyflow
  - axis: surface
    value: storefront-ui
  - axis: user
    value: signed-in personal
anchors:
  - coordinate: /cart
  - coordinate: /account/saved-credit-cards
  - coordinate: /checkout/completed
  - coordinate: Mutation.createOrderFromCart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: theme=2.63.0-alpha.2681; store=B2B-store
    at: 2026-11-26T05:09:58.551Z
    by: session:779b94fc
    who: kutasinaelena
---
Signed-in personal user with no saved cards (/account/saved-credit-cards: 'You have not saved any credit cards yet'), one in-stock item, Shipping with a new US address and Fixed Rate (Ground). Selecting 'Bank card (Skyflow)' renders a 'Payment card' form (iframe fields Card number, Cardholder name, Expiration date, Security code) plus an UNCHECKED checkbox 'Save card for future payments. It's safe.'. Filled the sandbox Mastercard 5424 0000 0000 0015, ticked the checkbox, clicked Place order once: createOrderFromCart stayed pending for roughly 30 s, then the page went to /checkout/completed ('Order CO261126-00007 has been successfully submitted'). /account/saved-credit-cards then listed one card '•••• 0015, Expires 12/29' (the list shows the last four digits and expiry, not the brand). The checkbox was ticked in this run, so it is not established whether an order without it also saves the card.
