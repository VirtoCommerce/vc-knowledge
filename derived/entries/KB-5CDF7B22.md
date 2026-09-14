---
id: KB-5CDF7B22
subject: gql-type-inputconfirmemailtype
plane: derived-first
question: What fields does the GraphQL type `InputConfirmEmailType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputConfirmEmailType
    hash: 680a8e0b44de
  - coordinate: InputConfirmEmailType.token
    hash: 887bd860b6a6
  - coordinate: InputConfirmEmailType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputConfirmEmailType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `token` | `String!` | Confirm email token |
| `userId` | `String!` | User identifier |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputconfirmemailtype.json`.
