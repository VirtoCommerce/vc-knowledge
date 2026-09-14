---
id: KB-FBEFE19C
subject: gql-type-inputchangeorganizationlogocommandtype
plane: derived-first
question: What fields does the GraphQL type `InputChangeOrganizationLogoCommandType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputChangeOrganizationLogoCommandType
    hash: 61ef9e719170
  - coordinate: InputChangeOrganizationLogoCommandType.logoUrl
    hash: 49be59455a00
  - coordinate: InputChangeOrganizationLogoCommandType.organizationId
    hash: 556e05ebe1be
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputChangeOrganizationLogoCommandType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `logoUrl` | `String` | — |
| `organizationId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputchangeorganizationlogocommandtype.json`.
