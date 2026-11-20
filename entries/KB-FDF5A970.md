---
id: KB-FDF5A970
subject: The storefront product card's variations button counts the in-stock-and-buyable master plus in-stock-and-buyable variations when the Show in stock filter is on, and variations.length + 1 when it is off
plane: experiential
question: How does the storefront catalog product card compute the number on its variations button?
questions:
  - text: Why does the variations button show 1 for a product with one variation?
  - text: Does the variations button count include the master product?
  - text: Does Show in stock change the variations button count on a product card?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /search
  - coordinate: Query.products
evidence:
  - method: source
    deployment: vc-frontend@dev-2026-11-20
    at: 2026-11-20T07:49:39.467Z
    by: session:fcdb91ae
    who: kutasinaelena
---
In vc-frontend (dev branch, read 2026-11-20) client-app/shared/catalog/components/product-card.vue renders the button with $t('pages.catalog.variations_button', variationsCount), where variationsCount = productsFilters.inStock ? getPurchasableVariationsCount(product) : getVariationsCount(product) (client-app/shared/catalog/utilities/variations.ts). getVariationsCount = variations.length + 1 (the product is one of its own variations); getPurchasableVariationsCount = (master isInStock && isBuyable ? 1 : 0) + the variations with isInStock && isBuyable. The panel it opens searches productfamilyid:<id> is:product,variation with the same in-stock filter. So a master with exactly one variation reads 2 with the filter off, and 1 with it on only when exactly one of master/variation is purchasable; the en locale key is 'No variations | {count} variation | {count} variations'.
