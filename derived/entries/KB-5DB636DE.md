---
id: KB-5DB636DE
subject: gql-mutations-markallpushmessagesunread
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.markAllPushMessagesUnread`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.markAllPushMessagesUnread
    hash: 5be3b475d5d2
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.markAllPushMessagesUnread

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
markAllPushMessagesUnread(): Boolean
```

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-markallpushmessagesunread.json`.
