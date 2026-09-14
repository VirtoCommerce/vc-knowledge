---
id: KB-2A4C5DF0
subject: gql-type-searchproductfilterrangevalue
plane: derived-first
question: What fields does the GraphQL type `SearchProductFilterRangeValue` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: SearchProductFilterRangeValue
    hash: 20cce95f38ee
  - coordinate: SearchProductFilterRangeValue.includeLowerBound
    hash: 6cc6e1bcdd0e
  - coordinate: SearchProductFilterRangeValue.includeUpperBound
    hash: 9e3f5d12b401
  - coordinate: SearchProductFilterRangeValue.lower
    hash: 10622d099d02
  - coordinate: SearchProductFilterRangeValue.upper
    hash: 07d4aa247324
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# SearchProductFilterRangeValue

A GraphQL object type on this deployment's schema, carrying 4 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `includeLowerBound` | `Boolean!` | Indicates if the starting bound is included in the range |
| `includeUpperBound` | `Boolean!` | Indicates if the ending bound is included in the range |
| `lower` | `String` | The starting value of the range |
| `upper` | `String` | The ending value of the range |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-searchproductfilterrangevalue.json`.
