---
id: KB-E29FC3BD
subject: gql-mutations-sendverifyemail
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.sendVerifyEmail`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.sendVerifyEmail
    hash: 6bd9078a509b
  - coordinate: InputSendVerifyEmailType
    hash: c767fa0000cf
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.sendVerifyEmail

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
sendVerifyEmail(command: InputSendVerifyEmailType): Boolean
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputSendVerifyEmailType` | no | — |

Types in this signature: `InputSendVerifyEmailType` (INPUT_OBJECT) — `gql-type-inputsendverifyemailtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-sendverifyemail.json`.
