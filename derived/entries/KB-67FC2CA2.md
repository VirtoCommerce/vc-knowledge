---
id: KB-67FC2CA2
subject: gql-type-registercontacttype
plane: derived-first
question: What fields does the GraphQL type `RegisterContactType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: RegisterContactType
    hash: ef28111b1ac9
  - coordinate: RegisterContactType.about
    hash: 4eea246875b6
  - coordinate: RegisterContactType.address
    hash: 19fe93dc9854
  - coordinate: RegisterContactType.birthdate
    hash: fdd76fdebd5f
  - coordinate: RegisterContactType.createdBy
    hash: bc799538b7b4
  - coordinate: RegisterContactType.dynamicProperties
    hash: 3c0a7ca5c1e0
  - coordinate: RegisterContactType.firstName
    hash: 70c8a1eb9908
  - coordinate: RegisterContactType.id
    hash: 23d182c998e6
  - coordinate: RegisterContactType.lastName
    hash: 6c8bc6fd8b3a
  - coordinate: RegisterContactType.middleName
    hash: 25f2140c236d
  - coordinate: RegisterContactType.phoneNumber
    hash: 337d23eb7171
  - coordinate: RegisterContactType.status
    hash: 206caa392af0
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# RegisterContactType

A GraphQL object type on this deployment's schema, carrying 11 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `about` | `String` | — |
| `address` | `MemberAddressType` | — |
| `birthdate` | `Date` | — |
| `createdBy` | `String` | — |
| `dynamicProperties` | `[DynamicPropertyValueType]` | Contact's dynamic property values |
| `firstName` | `String!` | — |
| `id` | `String!` | — |
| `lastName` | `String!` | — |
| `middleName` | `String` | — |
| `phoneNumber` | `String` | — |
| `status` | `String` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-registercontacttype.json`.
