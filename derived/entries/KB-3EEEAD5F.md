---
id: KB-3EEEAD5F
subject: gql-query-contacts
plane: derived-first
question: What is the signature of the GraphQL query `Query.contacts`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.contacts
    hash: d35227850f0a
  - coordinate: ContactConnection
    hash: c70afe443d4d
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.contacts

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
contacts(after: String, first: Int, searchPhrase: String, sort: String): ContactConnection
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `after` | `String` | no | Only return edges after the specified cursor. |
| `first` | `Int` | no | Specifies the maximum number of edges to return, starting after the cursor specified by 'after', or the first number of edges if 'after' is not specified. |
| `searchPhrase` | `String` | no | This parameter applies a filter to the query results |
| `sort` | `String` | no | The sort expression |

Types in this signature: `ContactConnection` (OBJECT) — `gql-type-contactconnection`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-contacts.json`.
