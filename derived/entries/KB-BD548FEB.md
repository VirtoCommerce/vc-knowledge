---
id: KB-BD548FEB
subject: gql-mutations-createconfiguredlineitem
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.createConfiguredLineItem`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.createConfiguredLineItem
    hash: 8bec839553b7
  - coordinate: ConfigurationLineItemType
    hash: 27165763e07d
  - coordinate: InputCreateConfiguredLineItemCommand
    hash: 54add04a2c48
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.createConfiguredLineItem

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
createConfiguredLineItem(command: InputCreateConfiguredLineItemCommand!): ConfigurationLineItemType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputCreateConfiguredLineItemCommand!` | yes | — |

Types in this signature: `ConfigurationLineItemType` (OBJECT) — `gql-type-configurationlineitemtype`, `InputCreateConfiguredLineItemCommand` (INPUT_OBJECT) — `gql-type-inputcreateconfiguredlineitemcommand`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-createconfiguredlineitem.json`.
