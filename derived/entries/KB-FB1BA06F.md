---
id: KB-FB1BA06F
subject: gql-query-fulfillmentcenters
plane: derived-first
question: What is the signature of the GraphQL query `Query.fulfillmentCenters`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.fulfillmentCenters
    hash: 34bb70d18c0f
  - coordinate: FulfillmentCenterConnection
    hash: b023d42bb2bf
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.fulfillmentCenters

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
fulfillmentCenters(after: String, first: Int, fulfillmentCenterIds: [String], query: String, sort: String, storeId: String): FulfillmentCenterConnection
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `after` | `String` | no | Only return edges after the specified cursor. |
| `first` | `Int` | no | Specifies the maximum number of edges to return, starting after the cursor specified by 'after', or the first number of edges if 'after' is not specified. |
| `fulfillmentCenterIds` | `[String]` | no | Filter by FFC IDs |
| `query` | `String` | no | Search FFC by name |
| `sort` | `String` | no | The sort expression |
| `storeId` | `String` | no | Search FFCs attached to a store |

Types in this signature: `FulfillmentCenterConnection` (OBJECT) — `gql-type-fulfillmentcenterconnection`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-fulfillmentcenters.json`.
