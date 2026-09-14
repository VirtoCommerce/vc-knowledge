---
id: KB-DF37CD5D
subject: gql-type-createreviewcommandtype
plane: derived-first
question: What fields does the GraphQL type `CreateReviewCommandType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: CreateReviewCommandType
    hash: 581ffc706020
  - coordinate: CreateReviewCommandType.entityId
    hash: 62651539f39e
  - coordinate: CreateReviewCommandType.entityType
    hash: 5ef7dc289caf
  - coordinate: CreateReviewCommandType.imageUrls
    hash: a78f139ef03d
  - coordinate: CreateReviewCommandType.rating
    hash: cb6b63ada96f
  - coordinate: CreateReviewCommandType.review
    hash: 3535941ee488
  - coordinate: CreateReviewCommandType.storeId
    hash: f750fb11945a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# CreateReviewCommandType

A GraphQL input object type on this deployment's schema, carrying 6 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `entityId` | `String!` | — |
| `entityType` | `String!` | — |
| `imageUrls` | `[String]` | — |
| `rating` | `Int!` | — |
| `review` | `String!` | — |
| `storeId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-createreviewcommandtype.json`.
