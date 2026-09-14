---
id: KB-CEE5A301
subject: gql-type-fulfillmentcenteredge
plane: derived-first
question: What fields does the GraphQL type `FulfillmentCenterEdge` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: FulfillmentCenterEdge
    hash: 88460b363d71
  - coordinate: FulfillmentCenterEdge.cursor
    hash: 9062859f7ad8
  - coordinate: FulfillmentCenterEdge.node
    hash: 469357a1c52e
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# FulfillmentCenterEdge

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `cursor` | `String!` | A cursor for use in pagination |
| `node` | `FulfillmentCenterType` | The item at the end of the edge |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-fulfillmentcenteredge.json`.
