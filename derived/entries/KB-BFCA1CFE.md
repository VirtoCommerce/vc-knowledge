---
id: KB-BFCA1CFE
subject: gql-mutations-changepurchaseordernumber
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.changePurchaseOrderNumber`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.changePurchaseOrderNumber
    hash: 5a59e26ccf76
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputChangePurchaseOrderNumber
    hash: 5e3bac123442
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.changePurchaseOrderNumber

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
changePurchaseOrderNumber(command: InputChangePurchaseOrderNumber): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangePurchaseOrderNumber` | no | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputChangePurchaseOrderNumber` (INPUT_OBJECT) — `gql-type-inputchangepurchaseordernumber`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-changepurchaseordernumber.json`.
