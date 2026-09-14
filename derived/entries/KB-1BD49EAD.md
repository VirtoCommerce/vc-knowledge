---
id: KB-1BD49EAD
subject: gql-type-inputaddorupdateorderpaymenttype
plane: derived-first
question: What fields does the GraphQL type `InputAddOrUpdateOrderPaymentType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputAddOrUpdateOrderPaymentType
    hash: 3c265aa68164
  - coordinate: InputAddOrUpdateOrderPaymentType.orderId
    hash: 48001bdb7ad2
  - coordinate: InputAddOrUpdateOrderPaymentType.payment
    hash: 75644a224b95
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputAddOrUpdateOrderPaymentType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `orderId` | `String!` | Order ID |
| `payment` | `InputOrderPaymentType!` | Payment |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputaddorupdateorderpaymenttype.json`.
