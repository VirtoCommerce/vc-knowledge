---
id: KB-115FEFBE
subject: gql-type-inputapplicationuserlogintype
plane: derived-first
question: What fields does the GraphQL type `InputApplicationUserLoginType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputApplicationUserLoginType
    hash: 6517e571c91d
  - coordinate: InputApplicationUserLoginType.loginProvider
    hash: f46498400308
  - coordinate: InputApplicationUserLoginType.providerKey
    hash: 69c3f12d6822
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputApplicationUserLoginType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `loginProvider` | `String!` | — |
| `providerKey` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputapplicationuserlogintype.json`.
