---
id: KB-35CDB7BA
subject: gql-type-inputupdateorganizationtype
plane: derived-first
question: What fields does the GraphQL type `InputUpdateOrganizationType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputUpdateOrganizationType
    hash: 3d475720d670
  - coordinate: InputUpdateOrganizationType.addresses
    hash: cc6aac294872
  - coordinate: InputUpdateOrganizationType.dynamicProperties
    hash: f06faa535377
  - coordinate: InputUpdateOrganizationType.emails
    hash: 64e95f88b6a8
  - coordinate: InputUpdateOrganizationType.groups
    hash: 90edcad698ca
  - coordinate: InputUpdateOrganizationType.id
    hash: 23d182c998e6
  - coordinate: InputUpdateOrganizationType.memberType
    hash: 6bcfcd3180c0
  - coordinate: InputUpdateOrganizationType.name
    hash: 89b70478ea39
  - coordinate: InputUpdateOrganizationType.phones
    hash: eda2aff0f2a7
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputUpdateOrganizationType

A GraphQL input object type on this deployment's schema, carrying 8 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `addresses` | `[InputMemberAddressType]` | — |
| `dynamicProperties` | `[InputDynamicPropertyValueType]` | — |
| `emails` | `[String]` | — |
| `groups` | `[String]` | — |
| `id` | `String!` | — |
| `memberType` | `String` | — |
| `name` | `String` | — |
| `phones` | `[String]` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputupdateorganizationtype.json`.
