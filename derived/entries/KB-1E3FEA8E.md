---
id: KB-1E3FEA8E
subject: gql-type-countrytype
plane: derived-first
question: What fields does the GraphQL type `CountryType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: CountryType
    hash: 4f6c9c6528eb
  - coordinate: CountryType.id
    hash: 23d182c998e6
  - coordinate: CountryType.name
    hash: d212a4ea663c
  - coordinate: CountryType.regions
    hash: 5bb8dff63646
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# CountryType

A GraphQL object type on this deployment's schema, carrying 3 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `id` | `String!` | Code of country. For example 'USA'. |
| `name` | `String!` | Name of country. For example 'United States of America'. |
| `regions` | `[CountryRegionType!]!` | Country regions. |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-countrytype.json`.
