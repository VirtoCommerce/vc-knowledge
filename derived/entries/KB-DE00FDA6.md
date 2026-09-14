---
id: KB-DE00FDA6
subject: gql-query-contact
plane: derived-first
question: What is the signature of the GraphQL query `Query.contact`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Query
    platformVersion: 3.1007.26
anchors:
  - coordinate: Query.contact
    hash: 03b6fb03e37d
  - coordinate: ContactType
    hash: 8948ea5dfcea
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Query.contact

A GraphQL query field on the root type `Query`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
contact(id: String!, userId: String): ContactType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `id` | `String!` | yes | — |
| `userId` | `String` | no | — |

Types in this signature: `ContactType` (OBJECT) — `gql-type-contacttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-query-contact.json`.
