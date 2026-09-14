---
id: KB-CA4E6A82
subject: gql-query-product
plane: derived-first
question: What is the signature of the GraphQL query `Query.product`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.product
    hash: 32c3ec36608e
  - coordinate: Product
    hash: 3cedc42a5b25
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.product

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
product(cultureName: String, currencyCode: String, custom: String, id: String!, previousOutline: String, storeId: String!, userId: String): Product
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | Culture name ("en-US") |
| `currencyCode` | `String` | no | Currency code ("USD") |
| `custom` | `String` | no | Can be used for custom query parameters |
| `id` | `String!` | yes | id of the product |
| `previousOutline` | `String` | no | Previous outline |
| `storeId` | `String!` | yes | Store Id |
| `userId` | `String` | no | User Id |

Types in this signature: `Product` (OBJECT) — `gql-type-product`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-product.json`.
