---
id: KB-B85A6FC2
subject: coupons/add updates an existing coupon in place when its id is sent
plane: experiential
question: Can an existing coupon's expiration date be changed without deleting and re-adding it?
questions:
  - text: Can I change a coupon's expiration date without deleting and recreating it?
  - text: POST /api/marketing/promotions/coupons/add with an existing coupon id upsert
  - text: Is there a PUT endpoint for updating a coupon?
  - text: When does the coupon code uniqueness check apply on coupons/add?
concepts:
  - id: coupon
  - id: promotion
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/marketing/promotions/coupons/add
evidence:
  - method: observation
    deployment: vcst
    at: 2026-10-05T10:09:37.922Z
    by: session:51fcfb97
    who: kutasinaelena
---
POST /api/marketing/promotions/coupons/add is an upsert by id: posting an existing coupon object with its id and a new expirationDate updates that coupon in place (same id, single row, usage history kept). The code-uniqueness check applies only to coupons sent without an id. There is no separate PUT route for coupons.
