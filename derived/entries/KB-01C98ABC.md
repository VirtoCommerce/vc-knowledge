---
id: KB-01C98ABC
subject: gql-type-ordertaxdetailtype
plane: derived-first
question: What fields does the GraphQL type `OrderTaxDetailType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: OrderTaxDetailType
    hash: 6133aea14384
  - coordinate: OrderTaxDetailType.amount
    hash: c39319efac35
  - coordinate: OrderTaxDetailType.name
    hash: d212a4ea663c
  - coordinate: OrderTaxDetailType.rate
    hash: 167e1f194f94
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# OrderTaxDetailType

A GraphQL object type on this deployment's schema, carrying 3 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `amount` | `MoneyType!` | — |
| `name` | `String!` | — |
| `rate` | `MoneyType!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-ordertaxdetailtype.json`.
