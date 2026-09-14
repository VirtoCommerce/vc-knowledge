---
id: KB-4FAB6DB1
subject: gql-mutations-updaterole
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.updateRole`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.updateRole
    hash: 4407ced31cd7
  - coordinate: IdentityResultType
    hash: cfa01af7c9d3
  - coordinate: InputUpdateRoleType
    hash: 1ec6c78dae47
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.updateRole

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
updateRole(command: InputUpdateRoleType!): IdentityResultType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputUpdateRoleType!` | yes | — |

Types in this signature: `IdentityResultType` (OBJECT) — `gql-type-identityresulttype`, `InputUpdateRoleType` (INPUT_OBJECT) — `gql-type-inputupdateroletype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-updaterole.json`.
