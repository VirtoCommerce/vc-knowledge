---
id: KB-D6C3B99A
subject: gql-mutations-createwishlist
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.createWishlist`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.createWishlist
    hash: 7b6ac62272e8
  - coordinate: InputCreateWishlistType
    hash: 3134d8564526
  - coordinate: WishlistType
    hash: 1a845a7c89f2
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.createWishlist

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
createWishlist(command: InputCreateWishlistType!): WishlistType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputCreateWishlistType!` | yes | — |

Types in this signature: `InputCreateWishlistType` (INPUT_OBJECT) — `gql-type-inputcreatewishlisttype`, `WishlistType` (OBJECT) — `gql-type-wishlisttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-createwishlist.json`.
