---
id: KB-62E0B3CE
subject: gql-type-inputchangecartitemcommenttype
plane: derived-first
question: What fields does the GraphQL type `InputChangeCartItemCommentType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputChangeCartItemCommentType
    hash: bf2feb2e4f27
  - coordinate: InputChangeCartItemCommentType.cartId
    hash: f59bee92dfee
  - coordinate: InputChangeCartItemCommentType.cartName
    hash: a2693aff2a50
  - coordinate: InputChangeCartItemCommentType.cartType
    hash: 329d90701ee6
  - coordinate: InputChangeCartItemCommentType.comment
    hash: fe4152aa9241
  - coordinate: InputChangeCartItemCommentType.cultureName
    hash: fc312ccec25e
  - coordinate: InputChangeCartItemCommentType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputChangeCartItemCommentType.lineItemId
    hash: 9d009c9131e0
  - coordinate: InputChangeCartItemCommentType.storeId
    hash: f750fb11945a
  - coordinate: InputChangeCartItemCommentType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputChangeCartItemCommentType

A GraphQL input object type on this deployment's schema, carrying 9 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `comment` | `String!` | Comment |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `lineItemId` | `String!` | Line item Id |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputchangecartitemcommenttype.json`.
