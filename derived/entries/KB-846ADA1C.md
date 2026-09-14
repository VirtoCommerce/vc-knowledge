---
id: KB-846ADA1C
subject: gql-mutations-selectcartitems
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.selectCartItems`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.selectCartItems
    hash: d3bc6ad6dd23
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputChangeCartItemsSelectedType
    hash: f9be19770864
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.selectCartItems

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
selectCartItems(command: InputChangeCartItemsSelectedType): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangeCartItemsSelectedType` | no | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputChangeCartItemsSelectedType` (INPUT_OBJECT) — `gql-type-inputchangecartitemsselectedtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-selectcartitems.json`.
