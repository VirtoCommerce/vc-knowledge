---
id: KB-0FABE289
subject: gql-type-inputupdateroletype
plane: derived-first
question: What fields does the GraphQL type `InputUpdateRoleType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputUpdateRoleType
    hash: e28740a8fcbc
  - coordinate: InputUpdateRoleType.role
    hash: f35880fa0aeb
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputUpdateRoleType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `role` | `InputUpdateRoleInnerType!` | Role to update |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputupdateroletype.json`.
