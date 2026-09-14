---
id: KB-FBFEA951
subject: gql-mutations-removecartitems
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.removeCartItems`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.removeCartItems
    hash: 393b81cffbfe
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputRemoveItemsType
    hash: 224144f67de0
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.removeCartItems

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
removeCartItems(command: InputRemoveItemsType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputRemoveItemsType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputRemoveItemsType` (INPUT_OBJECT) — `gql-type-inputremoveitemstype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-removecartitems.json`.
