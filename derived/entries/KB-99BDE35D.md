---
id: KB-99BDE35D
subject: rest-api-rating
plane: derived-first
question: Which endpoints does this deployment serve under /api/rating, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.CustomerReviews
    version: 3.1000.0
anchors:
  - coordinate: POST /api/rating/calculateStore
    operationId: CustomerReviewsModuleRating_CalculateStore
    hash: 82dfa6d60ab5
  - coordinate: POST /api/rating/entityRating
    operationId: CustomerReviewsModuleRating_GetEntityRating
    hash: ba3c689e9e63
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/rating

2 operations under `/api/rating`, served by module `VirtoCommerce.CustomerReviews`, published under the tag "Rating and Reviews".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `POST /api/rating/calculateStore`<br>`CustomerReviewsModuleRating_CalculateStore` | — | — | 200 |
| `POST /api/rating/entityRating`<br>`CustomerReviewsModuleRating_GetEntityRating` | — | body application/json (optional) | `RatingEntityStoreDto[]` |

Module `VirtoCommerce.CustomerReviews` — Rating and Reviews: Enables customers to share their feedback on products and vendors after making a purchase, helping others make informed decisions.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-rating.json`.
