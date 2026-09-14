---
id: KB-B5E1C2CD
subject: gql-mutations-removeaddressfromfavorites
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.removeAddressFromFavorites`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.removeAddressFromFavorites
    hash: 467afb33a2fe
  - coordinate: RemoveAddressFromFavoritesCommandType
    hash: d58344be1d77
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.removeAddressFromFavorites

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
removeAddressFromFavorites(command: RemoveAddressFromFavoritesCommandType!): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `RemoveAddressFromFavoritesCommandType!` | yes | — |

Types in this signature: `RemoveAddressFromFavoritesCommandType` (INPUT_OBJECT) — `gql-type-removeaddressfromfavoritescommandtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-removeaddressfromfavorites.json`.
