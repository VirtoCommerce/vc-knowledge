---
id: KB-3C831EFC
subject: gql-type-pagecontextresponsetype
plane: derived-first
question: What fields does the GraphQL type `PageContextResponseType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: PageContextResponseType
    hash: 62f6eb725b32
  - coordinate: PageContextResponseType.slugInfo
    hash: e874908dfff7
  - coordinate: PageContextResponseType.store
    hash: 65da91ba26d2
  - coordinate: PageContextResponseType.user
    hash: 75e91c4a15ed
  - coordinate: PageContextResponseType.whiteLabelingSettings
    hash: 7eb1d10ae9f8
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# PageContextResponseType

A GraphQL object type on this deployment's schema, carrying 4 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `slugInfo` | `SlugInfoResponseType` | — |
| `store` | `StoreResponseType` | — |
| `user` | `UserType` | User info |
| `whiteLabelingSettings` | `WhiteLabelingSettingsType` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-pagecontextresponsetype.json`.
