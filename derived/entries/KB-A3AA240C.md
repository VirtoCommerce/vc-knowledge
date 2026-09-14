---
id: KB-A3AA240C
subject: gql-mutations-deleteusers
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.deleteUsers`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.deleteUsers
    hash: bd50e3db4a0c
  - coordinate: IdentityResultType
    hash: cfa01af7c9d3
  - coordinate: InputDeleteUserType
    hash: c16f30f32ee8
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.deleteUsers

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
deleteUsers(command: InputDeleteUserType!): IdentityResultType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputDeleteUserType!` | yes | — |

Types in this signature: `IdentityResultType` (OBJECT) — `gql-type-identityresulttype`, `InputDeleteUserType` (INPUT_OBJECT) — `gql-type-inputdeleteusertype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-deleteusers.json`.
