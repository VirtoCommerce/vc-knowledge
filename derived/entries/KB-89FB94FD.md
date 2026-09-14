---
id: KB-89FB94FD
subject: gql-type-configurationsectiontype
plane: derived-first
question: What fields does the GraphQL type `ConfigurationSectionType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: ConfigurationSectionType
    hash: 30d819bf69ff
  - coordinate: ConfigurationSectionType.allowCustomText
    hash: aced7120f3fb
  - coordinate: ConfigurationSectionType.allowTextOptions
    hash: bbb46bce0b50
  - coordinate: ConfigurationSectionType.description
    hash: d2a651ddaa3a
  - coordinate: ConfigurationSectionType.id
    hash: 23d182c998e6
  - coordinate: ConfigurationSectionType.isRequired
    hash: f332d6a6af19
  - coordinate: ConfigurationSectionType.name
    hash: 89b70478ea39
  - coordinate: ConfigurationSectionType.options
    hash: c6f2da1b9fc6
  - coordinate: ConfigurationSectionType.type
    hash: fc449b03ed47
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# ConfigurationSectionType

A GraphQL object type on this deployment's schema, carrying 8 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `allowCustomText` | `Boolean!` | Is custom text allowed for Text-type section |
| `allowTextOptions` | `Boolean!` | Is predefined text options allowed for Text-type section |
| `description` | `String` | Configuration section description |
| `id` | `String!` | Configuration section id |
| `isRequired` | `Boolean!` | Is configuration section required |
| `name` | `String` | Configuration section name |
| `options` | `[ConfigurationLineItemType]` | — |
| `type` | `String!` | Configuration section type. Possible values: 'Product', 'Text', 'File' |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-configurationsectiontype.json`.
