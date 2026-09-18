---
id: KB-6AA0D7FB
subject: fixed rate shipping method option pricing
plane: experiential
question: why does choosing a different delivery method not change the shipping cost?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
  - coordinate: OrderShipmentType.price
  - coordinate: OrderShipmentType.shipmentMethodOption
arrivesAt:
  - coordinate: GET /api/stores/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T07:51:15.206Z
    by: session:ea806a91
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-15T13:44:38.618Z
---

Because FixedRate prices per OPTION through a store setting, and an unset setting is free. The FixedRateShippingMethod provider embeds one Rate setting per option -- VirtoCommerce.Shipping.FixedRateShippingMethod.Ground.Rate and .Air.Rate -- and when a store has never configured them they carry value null with defaultValue 0. The storefront still offers every option as a selectable delivery method, so a shopper can pick between Fixed Rate (Ground) and Fixed Rate (Air) and watch the Shipping cost line stay at 0.00 for both; nothing warns that the choice is priceless rather than free-by-promotion. The choice is still RECORDED -- the placed shipment keeps shipmentMethodCode FixedRate and shipmentMethodOption Air -- so the fulfilment instruction survives even though the money does not. Where the money lands: the shipment's own price is the shipping charge, and with a single shipment the order's shippingSubTotal and shippingTotal equal it. Before treating a 0.00 shipping total as a bug, read the method's settings on the order's own shippingMethod object, which the order API returns inline with the shipment.
