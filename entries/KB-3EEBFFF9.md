---
id: KB-3EEBFFF9
subject: Query.customerReviews drops the keyword argument whenever filter is non-empty (pre-existing; same before and after the VCST-6251 fix)
plane: experiential
question: Does Query.customerReviews apply both keyword and filter when both are passed?
questions:
  - text: Why does customerReviews keyword not narrow results when a rating filter is set?
  - text: Can keyword and rating:[x TO y] be combined in the customerReviews query?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.customerReviews
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.CustomerReviews=3.1008.0-pr-86-6eca; store=B2B-store
    defect: suspected
    at: 2026-11-04T08:15:23.880Z
    by: session:7c4c844c
    who: Lenajava1
---
On a product with 10 Approved reviews, Query.customerReviews(storeId, entityId, entityType Product) with keyword "<text of one review title>" alone returned 1 review. With filter "rating:[1 TO 5]" plus the same keyword it returned all 10 (identical to the filter alone); "rating:[1 TO 3]" + keyword returned 5 (= the filter alone), and "rating:[4 TO 5]" + a keyword matching a rating-2 review returned the 5 rating 4-5 reviews instead of 0. Same on the pre-fix CustomerReviews 3.1008.0 (vcptcore: rating:[1 TO 5] + keyword -> 2 of 2), so the fix did not introduce it. The docs describe keyword as "filter reviews by matching text" and filter as narrowing further, so the keyword being silently dropped is suspected to be a defect.
