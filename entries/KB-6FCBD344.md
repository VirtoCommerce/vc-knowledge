---
id: KB-6FCBD344
subject: On vc-frontend-next 3.3.0 the built-in home '/' renders the 'Shop by category' and 'Popular with buyers' sections with headings only and no items, because the slugs and category it queries do not exist in the store's catalog
plane: experiential
question: Why are the 'Shop by category' and 'Popular with buyers' sections empty on the vc-frontend-next home page?
questions:
  - text: Does the vc-frontend-next demo home show category tiles and products?
  - text: What does GetSlugInfo return for the home page category permalinks on vc-frontend-next?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: vc-frontend-next
anchors:
  - coordinate: Query.slugInfo
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: virtostart
    build: 8f3b3fdd36b67c83
    conditions: theme=3.3.0; platform=3.1099.0; store=B2B-store
    defect: suspected
    at: 2026-11-17T11:41:16.542Z
    by: session:521b9a19
    who: Lenajava1
---
Guest, 1920x1080, '/' on theme 3.3.0. The built-in demo home renders hero, 'Shop by category' (link 'All categories' to /catalog), 'Popular with buyers' (link 'See all' to /category/83599552-...), 'Shop by supplier', 'Straightforward terms, real inventory', 'Monday morning, in numbers', 'Authorized distributor for' and 'Become a customer'. The two catalog-driven sections show only their heading and link. The page fires GetSlugInfo for fixed permalinks (consumer-electronics, phones-and-accessories, homes, home-appliances, printers, kitchen-supplies, products-with-options); each answers slugInfo.entityInfo null. SearchProducts with filter category.subtree:<catalog>/83599552-... returns totalCount 0. The real top categories of this store (Computers & Laptops, Storage & Memory, ...) are not used. Same result signed in.
