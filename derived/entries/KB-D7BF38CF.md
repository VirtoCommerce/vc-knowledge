---
id: KB-D7BF38CF
subject: gql-query-dynamicproperty
plane: derived-first
question: What is the signature of the GraphQL query `Query.dynamicProperty`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.dynamicProperty
    hash: 2578c1cfe9e5
  - coordinate: DynamicPropertyType
    hash: 0fe6819ff2a9
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.dynamicProperty

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
dynamicProperty(cultureName: String, idOrName: String!, objectType: String): DynamicPropertyType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | Culture name ("en-US") |
| `idOrName` | `String!` | yes | Id or name of the dynamic property |
| `objectType` | `String` | no | Object type of the dynamic property |

Types in this signature: `DynamicPropertyType` (OBJECT) — `gql-type-dynamicpropertytype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-dynamicproperty.json`.
