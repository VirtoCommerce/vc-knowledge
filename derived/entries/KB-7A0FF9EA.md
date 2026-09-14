---
id: KB-7A0FF9EA
subject: gql-type-inputdeletecontacttype
plane: derived-first
question: What fields does the GraphQL type `InputDeleteContactType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputDeleteContactType
    hash: eed12955616c
  - coordinate: InputDeleteContactType.contactId
    hash: 5ea04c5b9d3e
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputDeleteContactType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `contactId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputdeletecontacttype.json`.
