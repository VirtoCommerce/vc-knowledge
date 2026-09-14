---
id: KB-2ED7CEC0
subject: gql-mutations-addorupdatecartshipment
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.addOrUpdateCartShipment`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.addOrUpdateCartShipment
    hash: d045ddd83988
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputAddOrUpdateCartShipmentType
    hash: 2c269f9cfa74
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.addOrUpdateCartShipment

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
addOrUpdateCartShipment(command: InputAddOrUpdateCartShipmentType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputAddOrUpdateCartShipmentType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputAddOrUpdateCartShipmentType` (INPUT_OBJECT) — `gql-type-inputaddorupdatecartshipmenttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-addorupdatecartshipment.json`.
