---
id: KB-5ADDB760
subject: gql-type-fileuploadscopeoptionstype
plane: derived-first
question: What fields does the GraphQL type `FileUploadScopeOptionsType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: FileUploadScopeOptionsType
    hash: 9e0b7302aa15
  - coordinate: FileUploadScopeOptionsType.allowAnonymousUpload
    hash: e4d19bc58075
  - coordinate: FileUploadScopeOptionsType.allowedExtensions
    hash: 5818984f6b48
  - coordinate: FileUploadScopeOptionsType.maxFileSize
    hash: 9f6838be499d
  - coordinate: FileUploadScopeOptionsType.scope
    hash: 835795063253
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# FileUploadScopeOptionsType

A GraphQL object type on this deployment's schema, carrying 4 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `allowAnonymousUpload` | `Boolean!` | — |
| `allowedExtensions` | `[String]!` | — |
| `maxFileSize` | `Long!` | — |
| `scope` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-fileuploadscopeoptionstype.json`.
