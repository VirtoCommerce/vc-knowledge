---
id: KB-C8F7F4C6
subject: gql-query-canleavefeedback
plane: derived-first
question: What is the signature of the GraphQL query `Query.canLeaveFeedback`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.canLeaveFeedback
    hash: 8345e1b21299
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.canLeaveFeedback

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
canLeaveFeedback(entityId: String!, entityType: String!, storeId: String!): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `entityId` | `String!` | yes | — |
| `entityType` | `String!` | yes | — |
| `storeId` | `String!` | yes | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-canleavefeedback.json`.
