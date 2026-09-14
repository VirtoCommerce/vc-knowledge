---
id: KB-C8DA0D7D
subject: gql-type-inputchangeallcartitemsselectedtype
plane: derived-first
question: What fields does the GraphQL type `InputChangeAllCartItemsSelectedType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputChangeAllCartItemsSelectedType
    hash: 655ccbe5e456
  - coordinate: InputChangeAllCartItemsSelectedType.cartId
    hash: f59bee92dfee
  - coordinate: InputChangeAllCartItemsSelectedType.cartName
    hash: a2693aff2a50
  - coordinate: InputChangeAllCartItemsSelectedType.cartType
    hash: 329d90701ee6
  - coordinate: InputChangeAllCartItemsSelectedType.cultureName
    hash: fc312ccec25e
  - coordinate: InputChangeAllCartItemsSelectedType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputChangeAllCartItemsSelectedType.storeId
    hash: f750fb11945a
  - coordinate: InputChangeAllCartItemsSelectedType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputChangeAllCartItemsSelectedType

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

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputchangeallcartitemsselectedtype.json`.
