---
id: KB-AB609D4E
subject: gql-query-checkusernameuniqueness
plane: derived-first
question: What is the signature of the GraphQL query `Query.checkUsernameUniqueness`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.checkUsernameUniqueness
    hash: be6346a25483
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.checkUsernameUniqueness

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
checkUsernameUniqueness(username: String!): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `username` | `String!` | yes | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-checkusernameuniqueness.json`.
