---
id: KB-C0D849ED
subject: gql-query-pagecontext
plane: derived-first
question: What is the signature of the GraphQL query `Query.pageContext`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.pageContext
    hash: e81abec179c8
  - coordinate: PageContextResponseType
    hash: d3007d564854
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.pageContext

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
pageContext(cultureName: String, domain: String, organizationId: String, permalink: String, storeId: String, userId: String): PageContextResponseType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `cultureName` | `String` | no | — |
| `domain` | `String` | no | — |
| `organizationId` | `String` | no | — |
| `permalink` | `String` | no | — |
| `storeId` | `String` | no | — |
| `userId` | `String` | no | — |

Types in this signature: `PageContextResponseType` (OBJECT) — `gql-type-pagecontextresponsetype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-pagecontext.json`.
