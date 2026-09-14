---
id: KB-6DB0DC63
subject: gql-type-propertygroup
plane: derived-first
question: What fields does the GraphQL type `PropertyGroup` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: PropertyGroup
    hash: fb0edc060d6c
  - coordinate: PropertyGroup.description
    hash: d2a651ddaa3a
  - coordinate: PropertyGroup.displayOrder
    hash: 815a8272837c
  - coordinate: PropertyGroup.id
    hash: 23d182c998e6
  - coordinate: PropertyGroup.name
    hash: 89b70478ea39
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# PropertyGroup

A GraphQL object type on this deployment's schema, carrying 4 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `description` | `String` | The localized description of the property group. |
| `displayOrder` | `Int` | The display order of the property group. |
| `id` | `String!` | The unique ID of the property group. |
| `name` | `String` | The localized name of the property group. |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-propertygroup.json`.
