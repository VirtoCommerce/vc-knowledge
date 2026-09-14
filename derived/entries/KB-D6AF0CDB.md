---
id: KB-D6AF0CDB
subject: gql-mutations-updatememberdynamicproperties
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.updateMemberDynamicProperties`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.updateMemberDynamicProperties
    hash: eecd2c18d958
  - coordinate: InputUpdateMemberDynamicPropertiesType
    hash: 0183809eb8b7
  - coordinate: MemberType
    hash: 1f08e2fba0df
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.updateMemberDynamicProperties

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
updateMemberDynamicProperties(command: InputUpdateMemberDynamicPropertiesType!): MemberType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputUpdateMemberDynamicPropertiesType!` | yes | — |

Types in this signature: `InputUpdateMemberDynamicPropertiesType` (INPUT_OBJECT) — `gql-type-inputupdatememberdynamicpropertiestype`, `MemberType` (OBJECT) — `gql-type-membertype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-updatememberdynamicproperties.json`.
