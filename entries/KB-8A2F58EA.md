---
id: KB-8A2F58EA
subject: xAPI returnableItems caps returnableQuantity at deliveredQuantity, computed per line
plane: experiential
question: Is returnableItems returnableQuantity capped by deliveredQuantity or orderedQuantity, and is it per line?
questions:
  - text: Can a buyer return items that were ordered but not yet delivered?
  - text: Query.returnableItems returnableQuantity deliveredQuantity orderedQuantity
  - text: With orderedQuantity 10 and deliveredQuantity 4, what returnableQuantity does xAPI return?
  - text: Is returnableQuantity reported per order line or as one value per return?
concepts:
  - id: returnable-quantity
  - id: return
  - id: order-line-item
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.returnableItems
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T19:06:48.635Z
    by: session:0cfc9f97
    who: kutasinaelena
    build: df4b8991ac35ca53
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-12T08:33:18.855Z
    by: session:573e5582
    who: Lenajava1
    note: "2026-10-12: fresh orders with two fully delivered lines (40 and 25) report returnableQuantity 40 and 25, one row per line."
    build: c2465c1bff0b7268
    buildFrom: deploy-history
---
The xAPI returnableItems(orderId) query returns one row per order line; returnableQuantity equals the line's deliveredQuantity (an order line with orderedQuantity 10 and deliveredQuantity 4 reports returnableQuantity 4, isReturnable true), and two eligible lines on one order each carry their own returnableQuantity (5 and 3), not a single per-return value.
