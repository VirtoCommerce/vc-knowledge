---
id: KB-BCE4AFD1
subject: gql-query-pages
plane: derived-first
question: What is the signature of the GraphQL query `Query.pages`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.pages
    hash: 122f72bc5cc8
  - coordinate: PageConnection
    hash: 87b94fa2b4fc
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.pages

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
pages(after: String, cultureName: String, first: Int, keyword: String!, storeId: String!): PageConnection
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `after` | `String` | no | Only return edges after the specified cursor. |
| `cultureName` | `String` | no | The language for which all localized category data will be returned |
| `first` | `Int` | no | Specifies the maximum number of edges to return, starting after the cursor specified by 'after', or the first number of edges if 'after' is not specified. |
| `keyword` | `String!` | yes | The keyword parameter performs the full-text search |
| `storeId` | `String!` | yes | The store id where pages are searched |

Types in this signature: `PageConnection` (OBJECT) — `gql-type-pageconnection`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-pages.json`.
