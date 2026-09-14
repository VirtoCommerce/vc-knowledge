---
id: KB-BEA0DBE6
subject: gql-type-asset
plane: derived-first
question: What fields does the GraphQL type `Asset` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: Asset
    hash: fe50e32e05c7
  - coordinate: Asset.cultureName
    hash: fc312ccec25e
  - coordinate: Asset.description
    hash: d2a651ddaa3a
  - coordinate: Asset.group
    hash: 46dc5520b697
  - coordinate: Asset.id
    hash: 23d182c998e6
  - coordinate: Asset.mimeType
    hash: 74b0d2014b49
  - coordinate: Asset.name
    hash: 89b70478ea39
  - coordinate: Asset.relativeUrl
    hash: d4fb0fa12be7
  - coordinate: Asset.size
    hash: fc0dba10ad9f
  - coordinate: Asset.typeId
    hash: cecda86f7c46
  - coordinate: Asset.url
    hash: a6e93cecb2fe
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Asset

A GraphQL object type on this deployment's schema, carrying 10 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `cultureName` | `String` | Culture name |
| `description` | `String` | The description of the asset. |
| `group` | `String` | The group of the asset. |
| `id` | `String!` | The unique ID of the asset. |
| `mimeType` | `String` | The MIME type of the asset. |
| `name` | `String` | The name of the asset. |
| `relativeUrl` | `String` | The relative URL of the asset. |
| `size` | `Long!` | The size of the asset in bytes. |
| `typeId` | `String!` | The type ID of the asset. |
| `url` | `String!` | The URL of the asset. |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-asset.json`.
