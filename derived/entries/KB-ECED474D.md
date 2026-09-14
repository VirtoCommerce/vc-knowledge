---
id: KB-ECED474D
subject: gql-query-category
plane: derived-first
question: What is the signature of the GraphQL query `Query.category`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.category
    hash: 679656dfdc2b
  - coordinate: Category
    hash: 5816848a9bea
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.category

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
category(cultureName: String, currencyCode: String, id: String!, previousOutline: String, storeId: String!, userId: String): Category
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | Culture name ("en-US") |
| `currencyCode` | `String` | no | Currency code ("USD") |
| `id` | `String!` | yes | id of the category |
| `previousOutline` | `String` | no | Previous outline |
| `storeId` | `String!` | yes | Store Id |
| `userId` | `String` | no | User Id |

Types in this signature: `Category` (OBJECT) — `gql-type-category`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-category.json`.
