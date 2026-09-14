---
id: KB-A8E59E9A
subject: gql-type-inputchangecartcurrencytype
plane: derived-first
question: What fields does the GraphQL type `InputChangeCartCurrencyType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputChangeCartCurrencyType
    hash: 7d5e8152a6a7
  - coordinate: InputChangeCartCurrencyType.cartId
    hash: f59bee92dfee
  - coordinate: InputChangeCartCurrencyType.cartName
    hash: a2693aff2a50
  - coordinate: InputChangeCartCurrencyType.cartType
    hash: 329d90701ee6
  - coordinate: InputChangeCartCurrencyType.cultureName
    hash: fc312ccec25e
  - coordinate: InputChangeCartCurrencyType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputChangeCartCurrencyType.newCurrencyCode
    hash: 3fed8f3d3e46
  - coordinate: InputChangeCartCurrencyType.storeId
    hash: f750fb11945a
  - coordinate: InputChangeCartCurrencyType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputChangeCartCurrencyType

A GraphQL input object type on this deployment's schema, carrying 8 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `newCurrencyCode` | `String!` | Second cart currency |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputchangecartcurrencytype.json`.
