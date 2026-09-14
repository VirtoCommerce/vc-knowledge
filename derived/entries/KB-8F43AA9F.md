---
id: KB-8F43AA9F
subject: gql-query-sharedwishlist
plane: derived-first
question: What is the signature of the GraphQL query `Query.sharedWishlist`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.sharedWishlist
    hash: 58ac5ae7266f
  - coordinate: WishlistType
    hash: 1a845a7c89f2
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.sharedWishlist

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
sharedWishlist(sharingKey: String!): WishlistType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `sharingKey` | `String!` | yes | Sharing key |

Types in this signature: `WishlistType` (OBJECT) — `gql-type-wishlisttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-sharedwishlist.json`.
