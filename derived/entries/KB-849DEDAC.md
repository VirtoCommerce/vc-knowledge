---
id: KB-849DEDAC
subject: gql-type-paymentinconnection
plane: derived-first
question: What fields does the GraphQL type `PaymentInConnection` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: PaymentInConnection
    hash: dfaed147fcfa
  - coordinate: PaymentInConnection.edges
    hash: 9df36c1a86fc
  - coordinate: PaymentInConnection.items
    hash: b3b29df2a917
  - coordinate: PaymentInConnection.pageInfo
    hash: c11711291823
  - coordinate: PaymentInConnection.totalCount
    hash: d42ece33d253
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# PaymentInConnection

A GraphQL object type on this deployment's schema, carrying 4 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `edges` | `[PaymentInEdge]` | A list of all of the edges returned in the connection. |
| `items` | `[PaymentInType]` | A list of all of the objects returned in the connection. This is a convenience field provided for quickly exploring the API; rather than querying for "{ edges { node } }" when no edge data is needed, this field can be used instead. Note that when clients like Relay need to fetch the "cursor" field on the edge to enable efficient pagination, this shortcut cannot be used, and the full "{ edges { node } } " version should be used instead. |
| `pageInfo` | `PageInfo!` | Information to aid in pagination. |
| `totalCount` | `Int` | A count of the total number of objects in this connection, ignoring pagination. This allows a client to fetch the first five objects by passing "5" as the argument to `first`, then fetch the total count so it could display "5 of 83", for example. In cases where we employ infinite scrolling or don't have an exact count of entries, this field will return `null`. |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-paymentinconnection.json`.
