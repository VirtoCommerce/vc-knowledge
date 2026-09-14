---
id: KB-FEBDB2DB
subject: gql-type-productedge
plane: derived-first
question: What fields does the GraphQL type `ProductEdge` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: ProductEdge
    hash: 3957921340ae
  - coordinate: ProductEdge.cursor
    hash: 9062859f7ad8
  - coordinate: ProductEdge.node
    hash: e2f6de7bb057
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# ProductEdge

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `cursor` | `String!` | A cursor for use in pagination |
| `node` | `Product` | The item at the end of the edge |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-productedge.json`.
