---
id: KB-2A9C78AA
subject: gql-mutations-changewishlist
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.changeWishlist`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.changeWishlist
    hash: bfe46ef7dda1
  - coordinate: InputChangeWishlistType
    hash: 8200e970dfc7
  - coordinate: WishlistType
    hash: 1a845a7c89f2
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.changeWishlist

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
changeWishlist(command: InputChangeWishlistType!): WishlistType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangeWishlistType!` | yes | — |

Types in this signature: `InputChangeWishlistType` (INPUT_OBJECT) — `gql-type-inputchangewishlisttype`, `WishlistType` (OBJECT) — `gql-type-wishlisttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-changewishlist.json`.
