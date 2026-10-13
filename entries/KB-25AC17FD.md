---
id: KB-25AC17FD
subject: addItem below a product's minQuantity succeeds as a mutation but adds no line; the rejection is only in cart.validationErrors (PRODUCT_MIN_QTY)
plane: experiential
question: Why does Mutation.addItem return no error yet the product never appears in the cart?
status: active
appliesTo:
  - axis: surface
    value: graphql
anchors:
  - coordinate: Mutation.addItem
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: module:VirtoCommerce.XCart=3.1041.0; store=B2B-store
    at: 2026-10-13T14:08:04.728Z
    by: session:aa76d86f
    who: Lenajava1
    build: 942f354b4df8bb56
    buildFrom: deploy-history
---
addItem(quantity: 1) for a product whose minQuantity is 100 returned the cart with no GraphQL error, no new line, and validationErrors [{ errorCode: PRODUCT_MIN_QTY, "quantity 1 is less than minimum 100" }]. A seeder or test that checks only for GraphQL errors reads this as success. 18 of 55 randomly discovered in-stock products on the stand had minQuantity above 1, so a discovery used to pick cart products must constrain minQuantity / packSize.
