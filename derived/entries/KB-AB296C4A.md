---
id: KB-AB296C4A
subject: gql-subscriptions-pushmessagecreated
plane: derived-first
question: What is the signature of the GraphQL subscription `Subscriptions.pushMessageCreated`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Subscriptions
    platformVersion: 3.1007.26
anchors:
  - coordinate: Subscriptions.pushMessageCreated
    hash: a6a66508a6c5
  - coordinate: PushMessageType
    hash: f30e9387263d
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Subscriptions.pushMessageCreated

A GraphQL subscription field on the root type `Subscriptions`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
pushMessageCreated(): PushMessageType!
```

Types in this signature: `PushMessageType` (OBJECT) — `gql-type-pushmessagetype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-subscriptions-pushmessagecreated.json`.
