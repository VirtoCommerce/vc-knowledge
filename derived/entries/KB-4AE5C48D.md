---
id: KB-4AE5C48D
subject: gql-query-payments
plane: derived-first
question: What is the signature of the GraphQL query `Query.payments`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.payments
    hash: 751864b5029c
  - coordinate: PaymentInConnection
    hash: 64fd06ada0a0
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.payments

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
payments(after: String, cultureName: String, facet: String, filter: String, first: Int, sort: String, userId: String): PaymentInConnection
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

Types in this signature: `PaymentInConnection` (OBJECT) — `gql-type-paymentinconnection`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-payments.json`.
