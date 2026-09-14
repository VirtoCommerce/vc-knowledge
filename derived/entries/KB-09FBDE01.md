---
id: KB-09FBDE01
subject: gql-mutations-deletefcmtoken
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.deleteFcmToken`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.deleteFcmToken
    hash: b99f6785ec3f
  - coordinate: InputDeleteFcmTokenType
    hash: 25da987dc0a8
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.deleteFcmToken

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
deleteFcmToken(command: InputDeleteFcmTokenType!): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputDeleteFcmTokenType!` | yes | — |

Types in this signature: `InputDeleteFcmTokenType` (INPUT_OBJECT) — `gql-type-inputdeletefcmtokentype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-deletefcmtoken.json`.
