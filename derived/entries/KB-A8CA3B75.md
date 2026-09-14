---
id: KB-A8CA3B75
subject: gql-mutations-updateorderitemdynamicproperties
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.updateOrderItemDynamicProperties`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.updateOrderItemDynamicProperties
    hash: 17533e6ca1d3
  - coordinate: CustomerOrderType
    hash: 15bd3945be9c
  - coordinate: InputUpdateOrderItemDynamicPropertiesType
    hash: 64d8082b5b40
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.updateOrderItemDynamicProperties

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
updateOrderItemDynamicProperties(command: InputUpdateOrderItemDynamicPropertiesType!): CustomerOrderType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputUpdateOrderItemDynamicPropertiesType!` | yes | — |

Types in this signature: `CustomerOrderType` (OBJECT) — `gql-type-customerordertype`, `InputUpdateOrderItemDynamicPropertiesType` (INPUT_OBJECT) — `gql-type-inputupdateorderitemdynamicpropertiestype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-updateorderitemdynamicproperties.json`.
