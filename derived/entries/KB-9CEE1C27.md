---
id: KB-9CEE1C27
subject: gql-query-fileuploadoptions
plane: derived-first
question: What is the signature of the GraphQL query `Query.fileUploadOptions`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.fileUploadOptions
    hash: b82df08b9f00
  - coordinate: FileUploadScopeOptionsType
    hash: ca9f42282bfc
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.fileUploadOptions

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
fileUploadOptions(scope: String): FileUploadScopeOptionsType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `scope` | `String` | no | — |

Types in this signature: `FileUploadScopeOptionsType` (OBJECT) — `gql-type-fileuploadscopeoptionstype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-fileuploadoptions.json`.
