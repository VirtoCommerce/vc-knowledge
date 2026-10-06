---
id: KB-ED3A631C
subject: a discount row's label is the promotion description verbatim and is not kept in step with the reward
plane: experiential
question: if I change a promotion's reward, does the label the shopper reads change with it?
questions:
  - text: Why does my basket say ten percent off when the amount taken off is twenty percent?
  - text: Where does the text on a cart discount row come from, and is it regenerated when the reward changes?
  - text: What does a shopper see on a discount line when the promotion has no description?
  - text: Can the storefront show a promotion's name, or only its description, through DiscountType?
  - text: Can an internal note typed into a promotion's description leak to customers on their order page?
concepts:
  - id: discount
  - id: promotion
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: DiscountType.description
  - coordinate: PUT /api/marketing/promotions
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T13:35:43.058Z
    by: session:f9446010
    splitFrom: KB-F027283D
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:25:57.201Z
    by: session:26f59771
    note: "The shopper reads the promotion description verbatim. On /account/orders/1c458e1f the Discount line expands to \"KB-LAB run13 (disposable, agent-created): 15% off the cart subtotal. Safe to disable/delete.\" - an internal note written by whoever created the promotion, rendered to the customer. The same string is in the order REST payload as discounts[0].description. A second leak of the same kind on the same page: the operator free-text cancellation reason is printed under the order status."
    splitFrom: KB-F027283D
---
No. The label on a cart's or an order's discount row is the promotion's DESCRIPTION field verbatim -- free text an operator types once -- and nothing derives it from the reward or checks it against one. Editing the reward rewrites the money and leaves the sentence alone, in the same row: after taking one promotion from 10 percent to 20 percent and changing nothing else, the cart's expanded Discount breakdown read 'KB-LAB run12 (disposable): 10% off the cart subtotal.' beside -63.80, which is 20 percent of the 319.00 subtotal. The two came from one live read, so this is not a stale page -- it is two fields of one record that are allowed to disagree. The description is also OPTIONAL: a promotion saved with a null description renders a discount row with an EMPTY label and only an amount, so the shopper is shown money off and no reason at all (seen on a pre-existing promotion giving a flat 50.00). Since the storefront can reach the promotion's NAME nowhere -- DiscountType carries description and promotionId but no name -- the description is the ONLY human-readable thing a shopper is ever shown about a promotion, on the cart and, snapshotted, on the order forever. The shopper reads it verbatim: on an order page the Discount line expands to an internal note written by whoever created the promotion, and the same string is in the order REST payload as discounts[0].description. Treat it as a published price claim: change a reward and you must change the sentence in the same save, and never leave it empty.
