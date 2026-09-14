---
id: KB-674B9AAE
subject: gql-mutations-addgiftitems
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.addGiftItems`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.addGiftItems
    hash: 4c006e1a3a14
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputAddGiftItemsType
    hash: 421b461a0b40
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.addGiftItems

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
addGiftItems(command: InputAddGiftItemsType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputAddGiftItemsType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputAddGiftItemsType` (INPUT_OBJECT) — `gql-type-inputaddgiftitemstype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-addgiftitems.json`.
