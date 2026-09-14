---
id: KB-D8FEA0F9
subject: gql-type-inputremovecarttype
plane: derived-first
question: What fields does the GraphQL type `InputRemoveCartType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputRemoveCartType
    hash: 787201ad0e8a
  - coordinate: InputRemoveCartType.cartId
    hash: 7d5131cdca30
  - coordinate: InputRemoveCartType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputRemoveCartType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String!` | Cart Id |
| `userId` | `String!` | User Id |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputremovecarttype.json`.
