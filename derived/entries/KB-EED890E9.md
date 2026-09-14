---
id: KB-EED890E9
subject: gql-query-pickuplocations
plane: derived-first
question: What is the signature of the GraphQL query `Query.pickupLocations`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.pickupLocations
    hash: 1d5a08352670
  - coordinate: PickupLocationConnection
    hash: f49dc5187f44
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.pickupLocations

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
pickupLocations(after: String, first: Int, keyword: String, sort: String, storeId: String): PickupLocationConnection
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `after` | `String` | no | Only return edges after the specified cursor. |
| `first` | `Int` | no | Specifies the maximum number of edges to return, starting after the cursor specified by 'after', or the first number of edges if 'after' is not specified. |
| `keyword` | `String` | no | — |
| `sort` | `String` | no | — |
| `storeId` | `String` | no | — |

Types in this signature: `PickupLocationConnection` (OBJECT) — `gql-type-pickuplocationconnection`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-pickuplocations.json`.
