---
id: KB-6BBC4FA7
subject: gql-type-inputkeyvaluetype
plane: derived-first
question: What fields does the GraphQL type `InputKeyValueType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputKeyValueType
    hash: b785a0abef7c
  - coordinate: InputKeyValueType.key
    hash: 8356a9b6042d
  - coordinate: InputKeyValueType.value
    hash: cd62057d405f
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputKeyValueType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `key` | `String!` | Dictionary key |
| `value` | `String` | Dictionary value |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputkeyvaluetype.json`.
