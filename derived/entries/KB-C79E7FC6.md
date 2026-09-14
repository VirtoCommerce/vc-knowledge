---
id: KB-C79E7FC6
subject: gql-mutations-removeshipment
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.removeShipment`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.removeShipment
    hash: 0f72a38cab65
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputRemoveShipmentType
    hash: afa7c2fd9216
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.removeShipment

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
removeShipment(command: InputRemoveShipmentType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputRemoveShipmentType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputRemoveShipmentType` (INPUT_OBJECT) — `gql-type-inputremoveshipmenttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-removeshipment.json`.
