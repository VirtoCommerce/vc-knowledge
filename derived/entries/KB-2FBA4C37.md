---
id: KB-2FBA4C37
subject: gql-type-inputcreateusertype
plane: derived-first
question: What fields does the GraphQL type `InputCreateUserType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputCreateUserType
    hash: a1f4055420c6
  - coordinate: InputCreateUserType.applicationUser
    hash: aa2b07a31cde
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputCreateUserType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `applicationUser` | `InputCreateApplicationUserType!` | Application user to create |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputcreateusertype.json`.
