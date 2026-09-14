---
id: KB-C00FD04D
subject: gql-mutations-initializepayment
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.initializePayment`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.initializePayment
    hash: 01c2e0511f55
  - coordinate: InitializePaymentResultType
    hash: 3c826b752244
  - coordinate: InputInitializePaymentType
    hash: f48a05f30471
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.initializePayment

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
initializePayment(command: InputInitializePaymentType!): InitializePaymentResultType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputInitializePaymentType!` | yes | — |

Types in this signature: `InitializePaymentResultType` (OBJECT) — `gql-type-initializepaymentresulttype`, `InputInitializePaymentType` (INPUT_OBJECT) — `gql-type-inputinitializepaymenttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-initializepayment.json`.
