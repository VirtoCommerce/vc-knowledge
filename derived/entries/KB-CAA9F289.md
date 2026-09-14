---
id: KB-CAA9F289
subject: gql-query-getsavedforlater
plane: derived-first
question: What is the signature of the GraphQL query `Query.getSavedForLater`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.getSavedForLater
    hash: bda65a2f433b
  - coordinate: CartType
    hash: 8354850caced
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.getSavedForLater

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
getSavedForLater(cultureName: String, currencyCode: String, organizationId: String, storeId: String!, userId: String!): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | Culture name ("en-US") |
| `currencyCode` | `String` | no | Currency code ("USD") |
| `organizationId` | `String` | no | Organization Id |
| `storeId` | `String!` | yes | Store Id |
| `userId` | `String!` | yes | Customer Id |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-getsavedforlater.json`.
