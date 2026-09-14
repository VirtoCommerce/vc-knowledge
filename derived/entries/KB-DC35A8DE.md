---
id: KB-DC35A8DE
subject: gql-type-deletefilecommandtype
plane: derived-first
question: What fields does the GraphQL type `DeleteFileCommandType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: DeleteFileCommandType
    hash: d7ee488d041a
  - coordinate: DeleteFileCommandType.id
    hash: 23d182c998e6
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# DeleteFileCommandType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `id` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-deletefilecommandtype.json`.
