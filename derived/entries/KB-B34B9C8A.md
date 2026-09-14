---
id: KB-B34B9C8A
subject: gql-type-roletype
plane: derived-first
question: What fields does the GraphQL type `RoleType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: RoleType
    hash: b601d8579a20
  - coordinate: RoleType.description
    hash: d2a651ddaa3a
  - coordinate: RoleType.id
    hash: 23d182c998e6
  - coordinate: RoleType.name
    hash: d212a4ea663c
  - coordinate: RoleType.normalizedName
    hash: d570c78cb667
  - coordinate: RoleType.permissions
    hash: f7b8986d317e
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# RoleType

A GraphQL object type on this deployment's schema, carrying 5 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `description` | `String` | — |
| `id` | `String!` | — |
| `name` | `String!` | — |
| `normalizedName` | `String!` | — |
| `permissions` | `[String]!` | Permissions in Role |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-roletype.json`.
