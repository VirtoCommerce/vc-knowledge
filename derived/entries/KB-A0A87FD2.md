---
id: KB-A0A87FD2
subject: gql-mutations-changecartcurrency
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.changeCartCurrency`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.changeCartCurrency
    hash: b9d1f2f6bc15
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputChangeCartCurrencyType
    hash: dc00adfec944
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.changeCartCurrency

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
changeCartCurrency(command: InputChangeCartCurrencyType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangeCartCurrencyType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputChangeCartCurrencyType` (INPUT_OBJECT) — `gql-type-inputchangecartcurrencytype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-changecartcurrency.json`.
