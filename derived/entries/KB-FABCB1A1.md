---
id: KB-FABCB1A1
subject: gql-mutations-changecartconfigureditem
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.changeCartConfiguredItem`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.changeCartConfiguredItem
    hash: 2164073e9058
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputChangeCartConfiguredItemType
    hash: df93445e93ed
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.changeCartConfiguredItem

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
changeCartConfiguredItem(command: InputChangeCartConfiguredItemType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangeCartConfiguredItemType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputChangeCartConfiguredItemType` (INPUT_OBJECT) — `gql-type-inputchangecartconfigureditemtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-changecartconfigureditem.json`.
