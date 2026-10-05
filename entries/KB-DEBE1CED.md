---
id: KB-DEBE1CED
subject: A master product page's variant picker never makes it purchasable, even with every option chosen
plane: experiential
question: Does selecting every variant option on a master product page make it purchasable?
questions:
  - text: I picked every option on the product page but the price stays N/A and I can't buy it - why?
  - text: Why does a master product page stay unpurchasable even when its variations are in stock?
  - text: Does the variant picker grey out option combinations that have no real variation?
  - text: Why does the add button keep the label to select options after every variant group has a value?
concepts:
  - id: product-variation
  - id: product-page
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /configurable-caps-shirts/hat
  - coordinate: Product.variations
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-19T10:59:17.461Z
    by: session:local_de
    splitFrom: KB-4AE52041
---
Not necessarily. On /products-with-options/configurable-caps-shirts/hat ("Men's Adjustable Scholarship Hat Team Color", advertised as 6 variations, From $4.00) the PDP renders four variant groups - Color, size_shirts, Size_chart, Fabric (data-test-id="variant-picker--<group>--<value>"). Picking a Color alone re-renders the H1 to a concrete variation name ("Light Brown Clemson Hat"), proving the picker does resolve a product, yet the Price stays "N/A" and the add control stays a DISABLED button whose accessible name is "Select options to proceed" - and it still does after one value is selected in ALL FOUR groups (Brown / 55 / No size / Cotton). No option is ever disabled or greyed as unavailable, so the UI gives no way to tell a valid combination from an invalid one; the offered matrix is 6x4x5x4 = 480 combinations against only 6 real variations.
