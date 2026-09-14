---
id: KB-F3E5AC3F
subject: gql-mutations-unselectcartitems
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.unSelectCartItems`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.unSelectCartItems
    hash: 1626d5611312
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputChangeCartItemsSelectedType
    hash: f9be19770864
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.unSelectCartItems

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
unSelectCartItems(command: InputChangeCartItemsSelectedType): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangeCartItemsSelectedType` | no | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputChangeCartItemsSelectedType` (INPUT_OBJECT) — `gql-type-inputchangecartitemsselectedtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-unselectcartitems.json`.
