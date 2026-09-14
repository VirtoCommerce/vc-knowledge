---
id: KB-30F67DFF
subject: gql-mutations-markpushmessageunread
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.markPushMessageUnread`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.markPushMessageUnread
    hash: 6810a6f133e4
  - coordinate: InputMarkPushMessageUnreadType
    hash: 44769e17ec3b
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.markPushMessageUnread

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
markPushMessageUnread(command: InputMarkPushMessageUnreadType!): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputMarkPushMessageUnreadType!` | yes | — |

Types in this signature: `InputMarkPushMessageUnreadType` (INPUT_OBJECT) — `gql-type-inputmarkpushmessageunreadtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-markpushmessageunread.json`.
