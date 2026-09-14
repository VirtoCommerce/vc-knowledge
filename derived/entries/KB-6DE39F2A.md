---
id: KB-6DE39F2A
subject: gql-type-inputupdatepersonaldatatype
plane: derived-first
question: What fields does the GraphQL type `InputUpdatePersonalDataType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputUpdatePersonalDataType
    hash: b46abccbcb41
  - coordinate: InputUpdatePersonalDataType.personalData
    hash: 9347b4ff9385
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputUpdatePersonalDataType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `personalData` | `InputPersonalDataType!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputupdatepersonaldatatype.json`.
