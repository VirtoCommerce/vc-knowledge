---
id: KB-FC20F7FC
subject: gql-type-inputclearcarttype
plane: derived-first
question: What fields does the GraphQL type `InputClearCartType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputClearCartType
    hash: ad1350c9c900
  - coordinate: InputClearCartType.cartId
    hash: f59bee92dfee
  - coordinate: InputClearCartType.cartName
    hash: a2693aff2a50
  - coordinate: InputClearCartType.cartType
    hash: 329d90701ee6
  - coordinate: InputClearCartType.cultureName
    hash: fc312ccec25e
  - coordinate: InputClearCartType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputClearCartType.storeId
    hash: f750fb11945a
  - coordinate: InputClearCartType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputClearCartType

A GraphQL input object type on this deployment's schema, carrying 7 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputclearcarttype.json`.
