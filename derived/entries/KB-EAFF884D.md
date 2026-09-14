---
id: KB-EAFF884D
subject: gql-type-pickuplocationedge
plane: derived-first
question: What fields does the GraphQL type `PickupLocationEdge` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: PickupLocationEdge
    hash: 257d1de1caac
  - coordinate: PickupLocationEdge.cursor
    hash: 9062859f7ad8
  - coordinate: PickupLocationEdge.node
    hash: 64f198d76f2f
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# PickupLocationEdge

A GraphQL object type on this deployment's schema, carrying 2 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `cursor` | `String!` | A cursor for use in pagination |
| `node` | `PickupLocationType` | The item at the end of the edge |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-pickuplocationedge.json`.
