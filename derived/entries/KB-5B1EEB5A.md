---
id: KB-5B1EEB5A
subject: gql-type-taxdetailtype
plane: derived-first
question: What fields does the GraphQL type `TaxDetailType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: TaxDetailType
    hash: ec576638f514
  - coordinate: TaxDetailType.amount
    hash: c39319efac35
  - coordinate: TaxDetailType.name
    hash: 89b70478ea39
  - coordinate: TaxDetailType.price
    hash: 87c6a0ffb12e
  - coordinate: TaxDetailType.rate
    hash: 167e1f194f94
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# TaxDetailType

A GraphQL object type on this deployment's schema, carrying 4 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `amount` | `MoneyType!` | Amount |
| `name` | `String` | Name |
| `price` | `MoneyType!` | Price |
| `rate` | `MoneyType!` | Rate |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-taxdetailtype.json`.
