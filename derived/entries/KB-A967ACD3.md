---
id: KB-A967ACD3
subject: gql-mutations-resetpasswordbytoken
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.resetPasswordByToken`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.resetPasswordByToken
    hash: c42952d91733
  - coordinate: CustomIdentityResultType
    hash: c1b126f55031
  - coordinate: InputResetPasswordByTokenType
    hash: 4879b34b0c4a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.resetPasswordByToken

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
resetPasswordByToken(command: InputResetPasswordByTokenType): CustomIdentityResultType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputResetPasswordByTokenType` | no | — |

Types in this signature: `CustomIdentityResultType` (OBJECT) — `gql-type-customidentityresulttype`, `InputResetPasswordByTokenType` (INPUT_OBJECT) — `gql-type-inputresetpasswordbytokentype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-resetpasswordbytoken.json`.
