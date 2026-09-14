---
id: KB-34DE52FE
subject: gql-type-categoryedge
plane: derived-first
question: What fields does the GraphQL type `CategoryEdge` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: CategoryEdge
    hash: d12ef53b7a12
  - coordinate: CategoryEdge.cursor
    hash: 9062859f7ad8
  - coordinate: CategoryEdge.node
    hash: 8a723d3e6d65
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# CategoryEdge

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `cursor` | `String!` | A cursor for use in pagination |
| `node` | `Category` | The item at the end of the edge |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-categoryedge.json`.
