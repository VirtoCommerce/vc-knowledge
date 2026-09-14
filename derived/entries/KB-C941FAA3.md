---
id: KB-C941FAA3
subject: gql-mutations-additem
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.addItem`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.addItem
    hash: 6409d77dad8c
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputAddItemType
    hash: f5d6976bbc77
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.addItem

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
addItem(command: InputAddItemType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputAddItemType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputAddItemType` (INPUT_OBJECT) — `gql-type-inputadditemtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-additem.json`.
