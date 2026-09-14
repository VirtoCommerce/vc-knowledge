---
id: KB-D3AD507F
subject: gql-subscriptions-ping
plane: derived-first
question: What is the signature of the GraphQL subscription `Subscriptions.ping`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Subscriptions
    platformVersion: 3.1007.26
anchors:
  - coordinate: Subscriptions.ping
    hash: 347a15d518ca
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Subscriptions.ping

A GraphQL subscription field on the root type `Subscriptions`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
ping(): String
```

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-subscriptions-ping.json`.
