---
id: KB-EFAEC237
subject: gql-type-filterfacet
plane: derived-first
question: What fields does the GraphQL type `FilterFacet` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: FilterFacet
    hash: 638a68a69d47
  - coordinate: FilterFacet.count
    hash: fb0c07fe9c02
  - coordinate: FilterFacet.facetType
    hash: 54dca1c2dd13
  - coordinate: FilterFacet.label
    hash: ea2f9f2d97cb
  - coordinate: FilterFacet.name
    hash: d212a4ea663c
  - coordinate: FilterFacet.order
    hash: f8b39650779e
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# FilterFacet

A GraphQL object type on this deployment's schema, carrying 5 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `count` | `Int!` | The number of products matching the value specified in the filter facet expression |
| `facetType` | `FacetTypes!` | The three types of facets. Terms, Range, Filter |
| `label` | `String!` | Localized name of the facet. |
| `name` | `String!` | The key/name of the facet. |
| `order` | `Int` | Display order of the facet. |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-filterfacet.json`.
