---
id: KB-18BFAB8A
subject: gql-mutations-sendpasswordresetemail
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.sendPasswordResetEmail`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.sendPasswordResetEmail
    hash: 26601d567135
  - coordinate: SendPasswordResetEmailCommandType
    hash: a7f680234464
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.sendPasswordResetEmail

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
sendPasswordResetEmail(command: SendPasswordResetEmailCommandType!): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `SendPasswordResetEmailCommandType!` | yes | — |

Types in this signature: `SendPasswordResetEmailCommandType` (INPUT_OBJECT) — `gql-type-sendpasswordresetemailcommandtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-sendpasswordresetemail.json`.
