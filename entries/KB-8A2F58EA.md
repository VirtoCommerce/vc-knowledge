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
---
The xAPI returnableItems(orderId) query returns one row per order line; returnableQuantity equals the line's deliveredQuantity (an order line with orderedQuantity 10 and deliveredQuantity 4 reports returnableQuantity 4, isReturnable true), and two eligible lines on one order each carry their own returnableQuantity (5 and 3), not a single per-return value.
