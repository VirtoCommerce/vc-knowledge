---
id: KB-EFE8E10A
subject: skipProductSubscription now carries expectedNextOrderDate and refuses a stale skip, but changeProductSubscription(nextOrderDate) from a stale page has no such guard and silently undoes a skip made in another tab
plane: experiential
question: Does a stale subscription page still corrupt the delivery schedule after the stale-skip fix?
questions:
  - text: What happens when a buyer saves a new next delivery date on a subscription page opened before another tab skipped?
  - text: Does Skip next delivery on an out-of-date subscription page skip again?
  - text: Which product subscription mutations check the date the page showed?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /account/subscriptions/{id}
  - coordinate: Mutation.skipProductSubscription
  - coordinate: Mutation.changeProductSubscription
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 9dfbb57eacb0bf2c
    conditions: module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-79cf; theme=2.63.0-pr-2602-57ad; store=B2B-store
    defect: suspected
    at: 2026-11-17T10:35:48.923Z
    by: session:d9b8e380
    who: kutasinaelena
---
Storefront /account/subscriptions/{id}, one Active monthly subscription open in two tabs. Tab B changed the date (Dec 17 -> Dec 22); tab A, still showing Dec 17, pressed Skip next delivery: the mutation sent expectedNextOrderDate, the server refused, and the page refreshed with "This delivery was already changed, maybe in another tab...". Then tab B skipped (Jan 22 -> Feb 22) while tab A still showed Jan 22; tab A picked Jan 25 in the calendar and saved: changeProductSubscription sent only {subscriptionId, nextOrderDate:2027-01-25}, was accepted, "Changes saved.", nextOrderDate became Jan 25 with skipCount still 2 — the skip from the other tab was silently undone. Quantity-only saves send only the changed field, so they do not overwrite the date.
