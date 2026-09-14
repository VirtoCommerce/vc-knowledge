---
id: KB-FA4183CA
subject: gql-mutations-updatecartitemdynamicproperties
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.updateCartItemDynamicProperties`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.updateCartItemDynamicProperties
    hash: a45c8e2862d4
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputUpdateCartItemDynamicPropertiesType
    hash: a644d09dd408
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.updateCartItemDynamicProperties

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
updateCartItemDynamicProperties(command: InputUpdateCartItemDynamicPropertiesType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputUpdateCartItemDynamicPropertiesType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputUpdateCartItemDynamicPropertiesType` (INPUT_OBJECT) — `gql-type-inputupdatecartitemdynamicpropertiestype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-updatecartitemdynamicproperties.json`.
