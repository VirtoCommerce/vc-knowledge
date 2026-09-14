---
id: KB-2DFDD456
subject: gql-mutations-changecartitemsquantity
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.changeCartItemsQuantity`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.changeCartItemsQuantity
    hash: bb4ad57265b5
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputChangeCartItemsQuantityType
    hash: 406ecfbb8b57
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.changeCartItemsQuantity

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
changeCartItemsQuantity(command: InputChangeCartItemsQuantityType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangeCartItemsQuantityType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputChangeCartItemsQuantityType` (INPUT_OBJECT) — `gql-type-inputchangecartitemsquantitytype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-changecartitemsquantity.json`.
