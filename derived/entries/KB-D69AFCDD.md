---
id: KB-D69AFCDD
subject: gql-type-inputaddgiftitemstype
plane: derived-first
question: What fields does the GraphQL type `InputAddGiftItemsType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputAddGiftItemsType
    hash: 34e00773ae29
  - coordinate: InputAddGiftItemsType.cartId
    hash: f59bee92dfee
  - coordinate: InputAddGiftItemsType.cartName
    hash: a2693aff2a50
  - coordinate: InputAddGiftItemsType.cartType
    hash: 329d90701ee6
  - coordinate: InputAddGiftItemsType.cultureName
    hash: fc312ccec25e
  - coordinate: InputAddGiftItemsType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputAddGiftItemsType.ids
    hash: 04c5df4991e6
  - coordinate: InputAddGiftItemsType.storeId
    hash: f750fb11945a
  - coordinate: InputAddGiftItemsType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputAddGiftItemsType

A GraphQL input object type on this deployment's schema, carrying 8 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `ids` | `[String]!` | IDs of gift rewards to add to the cart |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputaddgiftitemstype.json`.
