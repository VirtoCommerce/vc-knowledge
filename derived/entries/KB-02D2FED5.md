---
id: KB-02D2FED5
subject: gql-type-seoinfo
plane: derived-first
question: What fields does the GraphQL type `SeoInfo` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: SeoInfo
    hash: f149f020ba4a
  - coordinate: SeoInfo.id
    hash: 23d182c998e6
  - coordinate: SeoInfo.imageAltDescription
    hash: f7f82ccfa7c4
  - coordinate: SeoInfo.isActive
    hash: 507a118a63bd
  - coordinate: SeoInfo.languageCode
    hash: f8a4816f660d
  - coordinate: SeoInfo.metaDescription
    hash: 3c5d781efa81
  - coordinate: SeoInfo.metaKeywords
    hash: 6e9a47b88d1d
  - coordinate: SeoInfo.name
    hash: 89b70478ea39
  - coordinate: SeoInfo.objectId
    hash: 0ef05291d3b9
  - coordinate: SeoInfo.objectType
    hash: 73e845197931
  - coordinate: SeoInfo.outline
    hash: cd3dd1daa717
  - coordinate: SeoInfo.pageTitle
    hash: f9bff057c72d
  - coordinate: SeoInfo.semanticUrl
    hash: d10905ad5349
  - coordinate: SeoInfo.storeId
    hash: 91cb1fda3072
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# SeoInfo

A GraphQL object type on this deployment's schema, carrying 13 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `id` | `String!` | — |
| `imageAltDescription` | `String` | — |
| `isActive` | `Boolean!` | — |
| `languageCode` | `String` | — |
| `metaDescription` | `String` | — |
| `metaKeywords` | `String` | — |
| `name` | `String` | — |
| `objectId` | `String!` | — |
| `objectType` | `String!` | — |
| `outline` | `String` | — |
| `pageTitle` | `String` | — |
| `semanticUrl` | `String!` | — |
| `storeId` | `String` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-seoinfo.json`.
