---
id: KB-880CC2AD
subject: gql-query-shipmentstatuses
plane: derived-first
question: What is the signature of the GraphQL query `Query.shipmentStatuses`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.shipmentStatuses
    hash: 2c26e2c51965
  - coordinate: LocalizedSettingResponseType
    hash: be6bb6588f95
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.shipmentStatuses

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
shipmentStatuses(cultureName: String): LocalizedSettingResponseType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | — |

Types in this signature: `LocalizedSettingResponseType` (OBJECT) — `gql-type-localizedsettingresponsetype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-shipmentstatuses.json`.
