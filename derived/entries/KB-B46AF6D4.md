---
id: KB-B46AF6D4
subject: gql-type-modulesettingstype
plane: derived-first
question: What fields does the GraphQL type `ModuleSettingsType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: ModuleSettingsType
    hash: 4b1c1f049e58
  - coordinate: ModuleSettingsType.moduleId
    hash: 1b8984d87c6c
  - coordinate: ModuleSettingsType.settings
    hash: 62c0a851a5c7
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# ModuleSettingsType

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `moduleId` | `String!` | — |
| `settings` | `[ModuleSettingType!]!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-modulesettingstype.json`.
