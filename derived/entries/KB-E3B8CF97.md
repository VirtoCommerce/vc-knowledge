---
id: KB-E3B8CF97
subject: gql-type-rangefacetstatistics
plane: derived-first
question: What fields does the GraphQL type `RangeFacetStatistics` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: RangeFacetStatistics
    hash: 31cb93e34616
  - coordinate: RangeFacetStatistics.max
    hash: fc278082d63f
  - coordinate: RangeFacetStatistics.min
    hash: eed75690f0de
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# RangeFacetStatistics

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `max` | `Float` | The maximum value in the range or across ranges. |
| `min` | `Float` | The minimum value in the range or across ranges. |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-rangefacetstatistics.json`.
