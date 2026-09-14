---
id: KB-25FCCC97
subject: rest-api-order-shipments
plane: derived-first
question: Which endpoints does this deployment serve under /api/order/shipments, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Orders
    version: 3.1000.4
anchors:
  - coordinate: PATCH /api/order/shipments/{id}
    operationId: OrderModuleShipments_PatchShipment
    hash: 44afd8675244
  - coordinate: POST /api/order/shipments
    operationId: OrderModuleShipments_UpdateShipment
    hash: 6559bae3ecab
  - coordinate: POST /api/order/shipments/search
    operationId: OrderModuleShipments_SearchOrderShipments
    hash: c230babe7db5
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/order/shipments

3 operations under `/api/order/shipments`, served by module `VirtoCommerce.Orders`, published under the tag "Order Management".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `PATCH /api/order/shipments/{id}`<br>`OrderModuleShipments_PatchShipment` | Partial update for the specified Shipment by id | `id` (path), body `Operation[]` (optional) | 204 |
| `POST /api/order/shipments`<br>`OrderModuleShipments_UpdateShipment` | — | body application/json (optional) | 200 |
| `POST /api/order/shipments/search`<br>`OrderModuleShipments_SearchOrderShipments` | Search shipments by given criteria | body application/json (optional) | `ShipmentSearchResult` |

Module `VirtoCommerce.Orders` — Order Management: Document based flexible order management system.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-order-shipments.json`.
