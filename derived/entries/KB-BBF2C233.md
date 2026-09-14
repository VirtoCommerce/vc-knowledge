---
id: KB-BBF2C233
subject: gql-type-pricessumtype
plane: derived-first
question: What fields does the GraphQL type `PricesSumType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: PricesSumType
    hash: ed6a7c82cbd1
  - coordinate: PricesSumType.discountTotal
    hash: 6226a5e9f0a1
  - coordinate: PricesSumType.total
    hash: 52359287f058
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# PricesSumType

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `discountTotal` | `MoneyType!` | Total discount amount |
| `total` | `MoneyType!` | Total price |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-pricessumtype.json`.
