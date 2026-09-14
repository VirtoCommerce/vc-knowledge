---
id: KB-43BEEB50
subject: gql-query-orders
plane: derived-first
question: What is the signature of the GraphQL query `Query.orders`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.orders
    hash: f8b6876bfce3
  - coordinate: CustomerOrderConnection
    hash: 6ddb7859455a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.orders

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
orders(after: String, cultureName: String, facet: String, filter: String, first: Int, sort: String, userId: String): CustomerOrderConnection
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `after` | `String` | no | Only return edges after the specified cursor. |
| `cultureName` | `String` | no | Culture name ("en-US") |
| `facet` | `String` | no | This parameter applies a facet to the query results |
| `filter` | `String` | no | This parameter applies a filter to the query results |
| `first` | `Int` | no | Specifies the maximum number of edges to return, starting after the cursor specified by 'after', or the first number of edges if 'after' is not specified. |
| `sort` | `String` | no | The sort expression |
| `userId` | `String` | no | — |

Types in this signature: `CustomerOrderConnection` (OBJECT) — `gql-type-customerorderconnection`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-orders.json`.
