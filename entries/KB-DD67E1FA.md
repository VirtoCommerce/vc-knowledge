---
id: KB-DD67E1FA
subject: With Return.ReturnEnabled off the storefront has no Returns page, but the returns queries still answer
plane: experiential
question: What does Return.ReturnEnabled off change on the storefront and in xAPI?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Return.ReturnEnabled
  - coordinate: /account/returns
  - coordinate: Query.returnableItems
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-06T17:53:24.904Z
    by: session:b6bd53fc
---
With Return.ReturnEnabled off for the store (its default), the storefront registers no Returns route or menu entry, so /account/returns is a 404 page after a reload, and returnableItems reports RETURNS_DISABLED. organizationReturns, returns and returnPolicy still answer (observed on Return 3.1005.0-pr-28-62f9 and theme 2.59.0-pr-2523).
