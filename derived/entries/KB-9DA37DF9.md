---
id: KB-9DA37DF9
subject: gql-mutations-changecartitemselected
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.changeCartItemSelected`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.changeCartItemSelected
    hash: 2b2164cddc54
  - coordinate: CartType
    hash: 8354850caced
  - coordinate: InputChangeCartItemSelectedType
    hash: 648cc5e5ab34
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.changeCartItemSelected

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
changeCartItemSelected(command: InputChangeCartItemSelectedType): CartType
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `InputChangeCartItemSelectedType` | no | — |

Types in this signature: `CartType` (OBJECT) — `gql-type-carttype`, `InputChangeCartItemSelectedType` (INPUT_OBJECT) — `gql-type-inputchangecartitemselectedtype`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-changecartitemselected.json`.
