---
id: KB-3AB42E0C
subject: Product minVariationPrice is the lowest effective price across ALL variation price rows including quantity tiers, returned with list = sale = actual
plane: experiential
question: How is Product.minVariationPrice computed in xAPI, and does it include tier prices?
questions:
  - text: Why is minVariationPrice lower than every variation's price.actual?
  - text: Why does minVariationPrice.list equal its sale amount?
status: active
appliesTo:
  - axis: field
    value: minvariationprice
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.product
  - coordinate: Query.products
  - coordinate: POST /api/pricing/evaluate
evidence:
  - method: observation
    deployment: vcst_qa
    build: f75b254d87d474d1
    conditions: module:VirtoCommerce.XCatalog=3.1027.0-pr-107-5ca9; module:VirtoCommerce.Pricing=3.1010.0; store=B2B-store; currency=USD
    at: 2026-11-18T10:27:34.645Z
    by: session:25a30bd6
    who: Lenajava1
---
Query.product(id) for master SP459017 (4 variations, each qty-1 price list 480 / sale 340, plus tiers at minQuantity 10 = 310 and 50 = 285, per POST /api/pricing/evaluate) returned minVariationPrice actual = list = sale = 285, while every variation's price.actual was 340. For master 54FT59 (variations list 12.75/sale 12 and list 19.95/sale 10) it returned 10 for list, sale and actual. Matches source: pricing ProductPriceDocumentBuilder.AddMinPrice indexes {Currency, Value = min EffectiveValue over all price rows} into __minVariationPrice, and xCatalog MinVariationPriceBinder maps Value to Price.List. Identical across 3 runs and between Query.products and Query.product.
