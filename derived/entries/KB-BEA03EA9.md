---
id: KB-BEA03EA9
subject: gql-type-pushmessagetype
plane: derived-first
question: What fields does the GraphQL type `PushMessageType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: PushMessageType
    hash: f65b9c68bcd6
  - coordinate: PushMessageType.createdDate
    hash: 31b8ce9ba5c9
  - coordinate: PushMessageType.id
    hash: 23d182c998e6
  - coordinate: PushMessageType.isHidden
    hash: bd79e5cded51
  - coordinate: PushMessageType.isRead
    hash: d6d5accb23c0
  - coordinate: PushMessageType.shortMessage
    hash: 79abd14525f7
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# PushMessageType

A GraphQL object type on this deployment's schema, carrying 5 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `createdDate` | `DateTime!` | — |
| `id` | `String!` | — |
| `isHidden` | `Boolean!` | — |
| `isRead` | `Boolean!` | — |
| `shortMessage` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-pushmessagetype.json`.
