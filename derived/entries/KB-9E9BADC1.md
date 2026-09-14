---
id: KB-9E9BADC1
subject: gql-type-keyvaluetype
plane: derived-first
question: What fields does the GraphQL type `KeyValueType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: KeyValueType
    hash: 092b3961e402
  - coordinate: KeyValueType.key
    hash: 8356a9b6042d
  - coordinate: KeyValueType.value
    hash: cd62057d405f
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# KeyValueType

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `key` | `String!` | Dictionary key |
| `value` | `String` | Dictionary value |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-keyvaluetype.json`.
