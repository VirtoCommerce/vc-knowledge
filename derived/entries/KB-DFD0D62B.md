---
id: KB-DFD0D62B
subject: gql-mutations-selectallcartitems
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.selectAllCartItems`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.selectAllCartItems
    hash: 49341c2f75e3
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputChangeAllCartItemsSelectedType
    hash: e0359f33e83a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.selectAllCartItems

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
selectAllCartItems(command: InputChangeAllCartItemsSelectedType): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangeAllCartItemsSelectedType` | no | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputChangeAllCartItemsSelectedType` (INPUT_OBJECT) — `gql-type-inputchangeallcartitemsselectedtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-selectallcartitems.json`.
