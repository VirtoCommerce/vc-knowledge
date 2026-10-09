---
id: KB-FA0604AD
subject: Mutation.createOrderFromCart for a one-line cart (FixedRate/Ground + DefaultManualPaymentMethod) takes 0.5 to 3.6 s client-side on vcptcore_qa with released XOrder 3.1014.0 / XCart 3.1039.0, median about 1.2 s
plane: experiential
question: How long does Mutation.createOrderFromCart take on a stand running the released XOrder and XCart modules?
status: active
appliesTo:
  - axis: auth
    value: storefront-buyer
  - axis: cart
    value: single-line-manual-payment
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.createOrderFromCart
  - coordinate: POST /graphql
evidence:
  - method: observation (client-side timing, n=6)
    deployment: vcptcore_qa
    at: 2026-10-09T11:24:48.754Z
    by: session:df9d131f
    who: Lenajava1
---
On 2026-10-09 11:20 to 11:22Z, the same probe as on vcst_qa ran here: a buyer token, a fresh named cart, one $0.99 in-stock product, FixedRate/Ground, DefaultManualPaymentMethod and the buyer address, with client-side timing over a warm connection. createOrderFromCart took 3583, 2246, 704 / 1782, 641, 503 ms over two batches of 3: median about 1.2 s, max 3.6 s. The first call of each batch was the slowest, and none of the six was under 500 ms. The `me` control took 129 to 527 ms, typically about 145. A `cart(cartName)` read took 468 to 652 ms. Stand versions: platform 3.1076.0, XOrder 3.1014.0, XCart 3.1039.0, FileExperienceApi 3.1006.0, Xapi 3.1026.0, with the other order-path modules at the same versions as vcst_qa. All orders returned status New with no errors[].
