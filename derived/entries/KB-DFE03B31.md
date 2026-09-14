---
id: KB-DFE03B31
subject: gql-type-inputupdateroleinnertype
plane: derived-first
question: What fields does the GraphQL type `InputUpdateRoleInnerType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputUpdateRoleInnerType
    hash: 93a22fc39bd1
  - coordinate: InputUpdateRoleInnerType.concurrencyStamp
    hash: f5e1c552913f
  - coordinate: InputUpdateRoleInnerType.description
    hash: d2a651ddaa3a
  - coordinate: InputUpdateRoleInnerType.id
    hash: 23d182c998e6
  - coordinate: InputUpdateRoleInnerType.name
    hash: d212a4ea663c
  - coordinate: InputUpdateRoleInnerType.permissions
    hash: 711f093b2760
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputUpdateRoleInnerType

A GraphQL input object type on this deployment's schema, carrying 5 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `concurrencyStamp` | `String` | Concurrency Stamp |
| `description` | `String` | Role description |
| `id` | `String!` | Role ID |
| `name` | `String!` | Role name |
| `permissions` | `[InputAssignPermissionType]!` | List of Role permissions |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputupdateroleinnertype.json`.
