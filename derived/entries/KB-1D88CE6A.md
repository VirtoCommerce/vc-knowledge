---
id: KB-1D88CE6A
subject: gql-type-inputchangepurchaseordernumber
plane: derived-first
question: What fields does the GraphQL type `InputChangePurchaseOrderNumber` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputChangePurchaseOrderNumber
    hash: 9c86bcd1395d
  - coordinate: InputChangePurchaseOrderNumber.cartId
    hash: f59bee92dfee
  - coordinate: InputChangePurchaseOrderNumber.cartName
    hash: a2693aff2a50
  - coordinate: InputChangePurchaseOrderNumber.cartType
    hash: 329d90701ee6
  - coordinate: InputChangePurchaseOrderNumber.cultureName
    hash: fc312ccec25e
  - coordinate: InputChangePurchaseOrderNumber.currencyCode
    hash: ca3e3446f113
  - coordinate: InputChangePurchaseOrderNumber.purchaseOrderNumber
    hash: 6a3d13e59240
  - coordinate: InputChangePurchaseOrderNumber.storeId
    hash: f750fb11945a
  - coordinate: InputChangePurchaseOrderNumber.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputChangePurchaseOrderNumber

A GraphQL input object type on this deployment's schema, carrying 8 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `purchaseOrderNumber` | `String` | Purchase Order Number |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputchangepurchaseordernumber.json`.
