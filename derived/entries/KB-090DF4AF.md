---
id: KB-090DF4AF
subject: gql-mutations-mergecart
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.mergeCart`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.mergeCart
    hash: 36c52d644ec2
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputMergeCartType
    hash: 41df1c0273a4
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.mergeCart

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
mergeCart(command: InputMergeCartType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputMergeCartType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputMergeCartType` (INPUT_OBJECT) — `gql-type-inputmergecarttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-mergecart.json`.
