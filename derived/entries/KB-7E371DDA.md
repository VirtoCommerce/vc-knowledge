---
id: KB-7E371DDA
subject: gql-type-inputresetpasswordbytokentype
plane: derived-first
question: What fields does the GraphQL type `InputResetPasswordByTokenType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputResetPasswordByTokenType
    hash: 3374b098b090
  - coordinate: InputResetPasswordByTokenType.newPassword
    hash: b74d34dc1106
  - coordinate: InputResetPasswordByTokenType.token
    hash: 887bd860b6a6
  - coordinate: InputResetPasswordByTokenType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputResetPasswordByTokenType

A GraphQL input object type on this deployment's schema, carrying 3 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `newPassword` | `String!` | New password according with system security policy |
| `token` | `String!` | User password reset token |
| `userId` | `String!` | User identifier |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputresetpasswordbytokentype.json`.
