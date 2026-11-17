---
id: KB-EF8D387B
subject: On vc-frontend-next 3.3.0 'Price ↑' (Query.products sort price-ascending) orders cards by each product's own price, while variation cards display the lower 'From' minVariationPrice, so the visible prices are not in ascending order
plane: experiential
question: Does the Price ↑ sort on a vc-frontend-next category page order cards by the price they display?
questions:
  - text: Why are variation 'From' prices out of order after sorting by price ascending?
  - text: What price does Query.products sort price-ascending use for products with variations?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: vc-frontend-next
anchors:
  - coordinate: /cables-docks-adapters
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: virtostart
    build: 8f3b3fdd36b67c83
    conditions: theme=3.3.0; store=B2B-store; role=guest
    defect: suspected
    at: 2026-11-17T12:33:30.428Z
    by: session:521b9a19
    who: Lenajava1
---
Guest on /cables-docks-adapters, theme 3.3.0, store B2B-store. After 'Price ↑' (URL ?sort=price-ascending) the first 12 cards display 8.35 From, 12.53 From, 29.99, 29.99, 24.99 From, 34.99, 40.99, 37.99 From, 40.99 From, 59.99, 48.99 From, 69.99. The SearchProducts response (sort price-ascending) lists the same products with price.actual 9.99, 14.99, 29.99, 29.99, 29.99, 34.99, 40.99, 44.99, 48.99, 59.99, 58.99, 69.99: the sort follows the product's own price, but a variation card shows minVariationPrice as 'From', so the visible order is broken (3 inversions in 12). One inversion also exists in price.actual itself (59.99 before 58.99). 'Price ↓' (?sort=price-descending) showed a non-increasing first 12 (624.99 ... 69.99).
