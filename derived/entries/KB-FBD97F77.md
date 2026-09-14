---
id: KB-FBD97F77
subject: gql-query-requestpasswordreset
plane: derived-first
question: What is the signature of the GraphQL query `Query.requestPasswordReset`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.requestPasswordReset
    hash: fc26848bd40a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.requestPasswordReset

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
requestPasswordReset(cultureName: String, loginOrEmail: String!, storeId: String, urlSuffix: String): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | — |
| `loginOrEmail` | `String!` | yes | — |
| `storeId` | `String` | no | — |
| `urlSuffix` | `String` | no | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-requestpasswordreset.json`.
