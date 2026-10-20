---
id: KB-55F5BDA3
subject: GA4 place_order on /checkout/completed sends value = order subtotal (excl. shipping and tax) with shipping and tax as separate params, not the order total
plane: experiential
question: Is the GA4 place_order value the order total or the subtotal excluding shipping and tax?
status: active
appliesTo:
  - axis: event
    value: place_order
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /checkout/completed
  - coordinate: window.dataLayer
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a
    at: 2026-10-20T15:12:10.480Z
    by: session:d992339e
    who: Lenajava1
---
Signed-in personal user, one 167.00 product plus an auto-gift, Fixed Rate (Ground) shipping 150.00, Manual payment, cart Total shown 380.40: window.dataLayer place_order had currency USD, value 167, shipping 150, tax 63.4, transaction_id set and items 2 (gift at price 0 plus the product). So value excludes shipping and tax and value+shipping+tax equals the displayed total. Consistent with BL-GA4-003 and the developer docs; a test asserting value equals the order total would be stale. begin_checkout on /cart load, by contrast, carried value 200.4 = cart Total (tax included, no shipping chosen yet).
