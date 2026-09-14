---
id: KB-2BAF6B7C
subject: gql-query-property
plane: derived-first
question: What is the signature of the GraphQL query `Query.property`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.property
    hash: 32532c257dff
  - coordinate: Property
    hash: a1f616a70284
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.property

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
property(cultureName: String, id: String!): Property
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | The language for which all localized property dictionary items will be returned |
| `id` | `String!` | yes | id of the property |

Types in this signature: `Property` (OBJECT) — `gql-type-property`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-property.json`.
