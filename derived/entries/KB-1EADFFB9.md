---
id: KB-1EADFFB9
subject: gql-type-inputaddorupdatecartshipmenttype
plane: derived-first
question: What fields does the GraphQL type `InputAddOrUpdateCartShipmentType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputAddOrUpdateCartShipmentType
    hash: bb3788e30f65
  - coordinate: InputAddOrUpdateCartShipmentType.cartId
    hash: f59bee92dfee
  - coordinate: InputAddOrUpdateCartShipmentType.cartName
    hash: a2693aff2a50
  - coordinate: InputAddOrUpdateCartShipmentType.cartType
    hash: 329d90701ee6
  - coordinate: InputAddOrUpdateCartShipmentType.cultureName
    hash: fc312ccec25e
  - coordinate: InputAddOrUpdateCartShipmentType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputAddOrUpdateCartShipmentType.shipment
    hash: 73ff089608c8
  - coordinate: InputAddOrUpdateCartShipmentType.storeId
    hash: f750fb11945a
  - coordinate: InputAddOrUpdateCartShipmentType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputAddOrUpdateCartShipmentType

A GraphQL input object type on this deployment's schema, carrying 8 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `shipment` | `InputShipmentType!` | Shipment |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputaddorupdatecartshipmenttype.json`.
