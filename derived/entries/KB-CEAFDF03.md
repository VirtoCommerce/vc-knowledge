---
id: KB-CEAFDF03
subject: gql-mutations-updatecontact
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.updateContact`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.updateContact
    hash: bdddf9c23d1d
  - coordinate: ContactType
    hash: 8948ea5dfcea
  - coordinate: InputUpdateContactType
    hash: 9944b337f3eb
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.updateContact

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
updateContact(command: InputUpdateContactType!): ContactType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputUpdateContactType!` | yes | — |

Types in this signature: `ContactType` (OBJECT) — `gql-type-contacttype`, `InputUpdateContactType` (INPUT_OBJECT) — `gql-type-inputupdatecontacttype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-updatecontact.json`.
