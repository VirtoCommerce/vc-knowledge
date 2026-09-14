---
id: KB-ED551D0A
subject: gql-type-inputcreateorganizationtype
plane: derived-first
question: What fields does the GraphQL type `InputCreateOrganizationType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputCreateOrganizationType
    hash: d91a913f2824
  - coordinate: InputCreateOrganizationType.addresses
    hash: cc6aac294872
  - coordinate: InputCreateOrganizationType.dynamicProperties
    hash: f06faa535377
  - coordinate: InputCreateOrganizationType.name
    hash: 89b70478ea39
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputCreateOrganizationType

A GraphQL input object type on this deployment's schema, carrying 3 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `addresses` | `[InputMemberAddressType]` | — |
| `dynamicProperties` | `[InputDynamicPropertyValueType]` | — |
| `name` | `String` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputcreateorganizationtype.json`.
