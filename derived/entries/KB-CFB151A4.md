---
id: KB-CFB151A4
subject: gql-type-dynamicpropertyedge
plane: derived-first
question: What fields does the GraphQL type `DynamicPropertyEdge` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: DynamicPropertyEdge
    hash: 5457907c7eb2
  - coordinate: DynamicPropertyEdge.cursor
    hash: 9062859f7ad8
  - coordinate: DynamicPropertyEdge.node
    hash: d7934b4d6286
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# DynamicPropertyEdge

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `cursor` | `String!` | A cursor for use in pagination |
| `node` | `DynamicPropertyType` | The item at the end of the edge |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-dynamicpropertyedge.json`.
