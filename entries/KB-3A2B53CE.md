---
id: KB-3A2B53CE
subject: The Sales Rep coupons card shows "Unlimited" for an uncapped coupon and "{left} of {total} uses left" otherwise, with no singular form ("1 of 1 uses left")
plane: experiential
question: What uses-left text does a coupon card on the Sales Rep hub coupons widget and /company/my-customers/:organizationId/coupons page show?
questions:
  - text: Does the rep coupons card say 'use left' or 'uses left' for one remaining use?
  - text: What does the rep coupons card show for an unlimited coupon?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}/coupons
  - coordinate: /company/my-customers/{id}
evidence:
  - method: observation
    deployment: mac_local
    conditions: module:VirtoCommerce.SalesRep=3.1015.0-local-feat/VCST-6158; theme=2.61.0-local-feat/VCST-6158-coupon-item-facade; culture=en-US
    defect: suspected
    at: 2026-10-21T19:13:39.715Z
    by: session:6eb5c164
---
Signed in as a sales rep in en-US, the customer profile 'Available coupons' widget and the 'All promotion coupons' page showed 'Unlimited' for coupons with maxUsesNumber 0, '5 of 5 uses left' and '3 of 3 uses left' for caps 5 and 3 with no uses, and '1 of 1 uses left' for a coupon capped at 1 with no uses. The plugin's en string is '{left} of {total} uses left' with no plural branch; the ticket mock shows '1 of 1 use left'. Remaining uses = maxUsesNumber - totalUsesCount from salesRepCoupons.
