---
id: KB-A1BF32EE
subject: gql-mutations-movefromsavedforlater
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.moveFromSavedForLater`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.moveFromSavedForLater
    hash: a7e73e30c6f0
  - coordinate: CartWithListType
    hash: e7480901407b
  - coordinate: InputSaveForLaterType
    hash: b4a59a00fc64
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.moveFromSavedForLater

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
moveFromSavedForLater(command: InputSaveForLaterType!): CartWithListType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputSaveForLaterType!` | yes | — |

Types in this signature: `CartWithListType` (OBJECT) — `gql-type-cartwithlisttype`, `InputSaveForLaterType` (INPUT_OBJECT) — `gql-type-inputsaveforlatertype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-movefromsavedforlater.json`.
