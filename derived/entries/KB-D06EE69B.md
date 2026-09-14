---
id: KB-D06EE69B
subject: gql-type-inputaddfcmtokentype
plane: derived-first
question: What fields does the GraphQL type `InputAddFcmTokenType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputAddFcmTokenType
    hash: 0e4c74062729
  - coordinate: InputAddFcmTokenType.token
    hash: 887bd860b6a6
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputAddFcmTokenType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `token` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputaddfcmtokentype.json`.
