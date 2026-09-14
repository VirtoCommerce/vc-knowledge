---
id: KB-49A2ABFE
subject: gql-mutations-clearcart
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.clearCart`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.clearCart
    hash: 8d600e417e6f
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputClearCartType
    hash: 0f841201d3e4
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.clearCart

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
clearCart(command: InputClearCartType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputClearCartType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputClearCartType` (INPUT_OBJECT) — `gql-type-inputclearcarttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-clearcart.json`.
