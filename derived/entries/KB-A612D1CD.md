---
id: KB-A612D1CD
subject: gql-type-inputregisterorganizationtype
plane: derived-first
question: What fields does the GraphQL type `InputRegisterOrganizationType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputRegisterOrganizationType
    hash: 892fea60adf6
  - coordinate: InputRegisterOrganizationType.address
    hash: 7700b6d2fc35
  - coordinate: InputRegisterOrganizationType.addresses
    hash: cc6aac294872
  - coordinate: InputRegisterOrganizationType.description
    hash: d2a651ddaa3a
  - coordinate: InputRegisterOrganizationType.dynamicProperties
    hash: f06faa535377
  - coordinate: InputRegisterOrganizationType.name
    hash: d212a4ea663c
  - coordinate: InputRegisterOrganizationType.phoneNumber
    hash: 337d23eb7171
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputRegisterOrganizationType

A GraphQL input object type on this deployment's schema, carrying 6 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `address` | `InputMemberAddressType` | — |
| `addresses` | `[InputMemberAddressType]` | — |
| `description` | `String` | — |
| `dynamicProperties` | `[InputDynamicPropertyValueType]` | — |
| `name` | `String!` | — |
| `phoneNumber` | `String` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputregisterorganizationtype.json`.
