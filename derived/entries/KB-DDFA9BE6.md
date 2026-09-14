---
id: KB-DDFA9BE6
subject: gql-type-inputdeletememberaddresstype
plane: derived-first
question: What fields does the GraphQL type `InputDeleteMemberAddressType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputDeleteMemberAddressType
    hash: 70122cff09d5
  - coordinate: InputDeleteMemberAddressType.addresses
    hash: c9bcd70c1966
  - coordinate: InputDeleteMemberAddressType.memberId
    hash: c459f4e82c27
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputDeleteMemberAddressType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `addresses` | `[InputMemberAddressType]!` | — |
| `memberId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputdeletememberaddresstype.json`.
