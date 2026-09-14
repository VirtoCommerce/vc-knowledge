---
id: KB-A87E9FE9
subject: gql-mutations-removememberfromorganization
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.removeMemberFromOrganization`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.removeMemberFromOrganization
    hash: 2349b1d6e4b9
  - coordinate: ContactType
    hash: 8948ea5dfcea
  - coordinate: InputRemoveMemberFromOrganizationType
    hash: 288ab281e03f
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.removeMemberFromOrganization

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
removeMemberFromOrganization(command: InputRemoveMemberFromOrganizationType!): ContactType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputRemoveMemberFromOrganizationType!` | yes | — |

Types in this signature: `ContactType` (OBJECT) — `gql-type-contacttype`, `InputRemoveMemberFromOrganizationType` (INPUT_OBJECT) — `gql-type-inputremovememberfromorganizationtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-removememberfromorganization.json`.
