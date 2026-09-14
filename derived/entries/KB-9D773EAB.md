---
id: KB-9D773EAB
subject: gql-query-validatepassword
plane: derived-first
question: What is the signature of the GraphQL query `Query.validatePassword`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.validatePassword
    hash: 74937c789942
  - coordinate: CustomIdentityResultType
    hash: c1b126f55031
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.validatePassword

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
validatePassword(password: String!): CustomIdentityResultType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `password` | `String!` | yes | — |

Types in this signature: `CustomIdentityResultType` (OBJECT) — `gql-type-customidentityresulttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-validatepassword.json`.
