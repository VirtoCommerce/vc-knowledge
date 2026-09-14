---
id: KB-CC6B1A17
subject: gql-type-refreshcarttype
plane: derived-first
question: What fields does the GraphQL type `RefreshCartType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: RefreshCartType
    hash: 58b684444233
  - coordinate: RefreshCartType.cartId
    hash: f59bee92dfee
  - coordinate: RefreshCartType.cartName
    hash: a2693aff2a50
  - coordinate: RefreshCartType.cartType
    hash: 329d90701ee6
  - coordinate: RefreshCartType.cultureName
    hash: fc312ccec25e
  - coordinate: RefreshCartType.currencyCode
    hash: ca3e3446f113
  - coordinate: RefreshCartType.storeId
    hash: f750fb11945a
  - coordinate: RefreshCartType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# RefreshCartType

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

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-refreshcarttype.json`.
