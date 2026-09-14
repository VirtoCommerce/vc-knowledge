---
id: KB-31FCE9A9
subject: gql-type-inputupdatememberaddresstype
plane: derived-first
question: What fields does the GraphQL type `InputUpdateMemberAddressType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputUpdateMemberAddressType
    hash: e4165a14014a
  - coordinate: InputUpdateMemberAddressType.addresses
    hash: c9bcd70c1966
  - coordinate: InputUpdateMemberAddressType.memberId
    hash: c459f4e82c27
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputUpdateMemberAddressType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `addresses` | `[InputMemberAddressType]!` | — |
| `memberId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputupdatememberaddresstype.json`.
