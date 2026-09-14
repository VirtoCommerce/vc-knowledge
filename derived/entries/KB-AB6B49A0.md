---
id: KB-AB6B49A0
subject: gql-query-page
plane: derived-first
question: What is the signature of the GraphQL query `Query.page`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.page
    hash: 798fb84ad8da
  - coordinate: PageType
    hash: b42d1f731266
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.page

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
page(cultureName: String, id: String!, storeId: String!): PageType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | — |
| `id` | `String!` | yes | — |
| `storeId` | `String!` | yes | — |

Types in this signature: `PageType` (OBJECT) — `gql-type-pagetype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-page.json`.
