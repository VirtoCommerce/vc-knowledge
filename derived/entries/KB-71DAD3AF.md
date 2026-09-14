---
id: KB-71DAD3AF
subject: gql-query-pushmessages
plane: derived-first
question: What is the signature of the GraphQL query `Query.pushMessages`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.pushMessages
    hash: e27587901b77
  - coordinate: PushMessageConnection
    hash: 9950b23dacd9
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.pushMessages

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
pushMessages(after: String, cultureName: String, first: Int, keyword: String, sort: String, unreadOnly: Boolean, withHidden: Boolean): PushMessageConnection
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `after` | `String` | no | Only return edges after the specified cursor. |
| `cultureName` | `String` | no | — |
| `first` | `Int` | no | Specifies the maximum number of edges to return, starting after the cursor specified by 'after', or the first number of edges if 'after' is not specified. |
| `keyword` | `String` | no | — |
| `sort` | `String` | no | — |
| `unreadOnly` | `Boolean` | no | — |
| `withHidden` | `Boolean` | no | — |

Types in this signature: `PushMessageConnection` (OBJECT) — `gql-type-pushmessageconnection`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-pushmessages.json`.
