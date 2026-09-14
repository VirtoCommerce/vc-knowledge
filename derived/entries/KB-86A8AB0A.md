---
id: KB-86A8AB0A
subject: gql-type-inputchangecommenttype
plane: derived-first
question: What fields does the GraphQL type `InputChangeCommentType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputChangeCommentType
    hash: 6dc7a37a3132
  - coordinate: InputChangeCommentType.cartId
    hash: f59bee92dfee
  - coordinate: InputChangeCommentType.cartName
    hash: a2693aff2a50
  - coordinate: InputChangeCommentType.cartType
    hash: 329d90701ee6
  - coordinate: InputChangeCommentType.comment
    hash: daa8fcab278e
  - coordinate: InputChangeCommentType.cultureName
    hash: fc312ccec25e
  - coordinate: InputChangeCommentType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputChangeCommentType.storeId
    hash: f750fb11945a
  - coordinate: InputChangeCommentType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputChangeCommentType

A GraphQL input object type on this deployment's schema, carrying 8 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `comment` | `String` | Comment |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputchangecommenttype.json`.
