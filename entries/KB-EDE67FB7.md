---
id: KB-EDE67FB7
subject: "On the sales rep customer profile, layout edits made in Edit layout are sent nowhere until Save layout: a reload or Cancel discards them, and Cancel re-mounts the blocks, which re-query their data"
plane: experiential
question: Are unsaved Edit layout changes on /company/my-customers/:organizationId persisted on reload or Cancel?
questions:
  - text: Does hiding a block in Edit layout save immediately?
  - text: What does Cancel do in the sales rep profile layout editor?
  - text: Does changing a block's maximum rows apply before Save layout?
concepts:
  - id: sales-rep
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
  - coordinate: Query.salesRepLayout
  - coordinate: Query.salesRepCoupons
evidence:
  - method: observation
    deployment: mac_local
    conditions: platform=3.1087.0; module:VirtoCommerce.SalesRep=3.1015.0-local; theme=2.61.0; store=B2B-store; role=Sales Representative
    at: 2026-10-21T19:25:55.646Z
    by: session:6eb5c164
---
Signed in as a sales rep with no saved layout, opened a served customer's profile, clicked Edit layout, set the Available coupons block's 'Maximum rows shown' spinbutton (1-20) from 5 to 2 and hid Top sellers (it became a 'Show Top sellers' chip). No GraphQL request was sent for either change, and the coupons block still showed 5 rows in edit mode. A reload showed Top sellers again and 5 coupon rows. In a second edit, Hide Available coupons followed by Cancel left edit mode with the block back, and the block re-sent SalesRepCoupons (first 5). salesRepLayout stayed null for that rep throughout. Edit mode shows Reset, Cancel and Save layout, plus Reorder, max rows and Hide per block.
