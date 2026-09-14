---
id: KB-8EC0DB91
subject: gql-mutations-updatecartpaymentdynamicproperties
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.updateCartPaymentDynamicProperties`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.updateCartPaymentDynamicProperties
    hash: bb8945d26ec1
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputUpdateCartPaymentDynamicPropertiesType
    hash: c7298f450d48
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.updateCartPaymentDynamicProperties

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
updateCartPaymentDynamicProperties(command: InputUpdateCartPaymentDynamicPropertiesType!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputUpdateCartPaymentDynamicPropertiesType!` | yes | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputUpdateCartPaymentDynamicPropertiesType` (INPUT_OBJECT) — `gql-type-inputupdatecartpaymentdynamicpropertiestype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-updatecartpaymentdynamicproperties.json`.
