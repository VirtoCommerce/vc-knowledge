---
id: KB-BCAB25A1
subject: gql-mutations-deletefile
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.deleteFile`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.deleteFile
    hash: 733b34d3834d
  - coordinate: DeleteFileCommandType
    hash: e0d997310cfa
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.deleteFile

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
deleteFile(command: DeleteFileCommandType!): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `DeleteFileCommandType!` | yes | — |

Types in this signature: `DeleteFileCommandType` (INPUT_OBJECT) — `gql-type-deletefilecommandtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-deletefile.json`.
