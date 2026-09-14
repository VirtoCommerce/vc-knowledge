---
id: KB-1C60E7AF
subject: gql-type-inputmarkpushmessageunreadtype
plane: derived-first
question: What fields does the GraphQL type `InputMarkPushMessageUnreadType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputMarkPushMessageUnreadType
    hash: d703c45f0917
  - coordinate: InputMarkPushMessageUnreadType.messageId
    hash: a096ae617443
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputMarkPushMessageUnreadType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `messageId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputmarkpushmessageunreadtype.json`.
