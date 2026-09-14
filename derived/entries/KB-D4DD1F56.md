---
id: KB-D4DD1F56
subject: gql-type-inputchangecartitempricetype
plane: derived-first
question: What fields does the GraphQL type `InputChangeCartItemPriceType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputChangeCartItemPriceType
    hash: cb7601ab7c11
  - coordinate: InputChangeCartItemPriceType.cartId
    hash: f59bee92dfee
  - coordinate: InputChangeCartItemPriceType.cartName
    hash: a2693aff2a50
  - coordinate: InputChangeCartItemPriceType.cartType
    hash: 329d90701ee6
  - coordinate: InputChangeCartItemPriceType.cultureName
    hash: fc312ccec25e
  - coordinate: InputChangeCartItemPriceType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputChangeCartItemPriceType.lineItemId
    hash: 9d009c9131e0
  - coordinate: InputChangeCartItemPriceType.price
    hash: 207d36dbfa92
  - coordinate: InputChangeCartItemPriceType.storeId
    hash: f750fb11945a
  - coordinate: InputChangeCartItemPriceType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputChangeCartItemPriceType

A GraphQL input object type on this deployment's schema, carrying 9 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `lineItemId` | `String!` | Line item Id |
| `price` | `Decimal!` | Price |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputchangecartitempricetype.json`.
