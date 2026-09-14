---
id: KB-A9F6E8A4
subject: gql-type-productconnection
plane: derived-first
question: What fields does the GraphQL type `ProductConnection` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: ProductConnection
    hash: 4d0a57684cff
  - coordinate: ProductConnection.edges
    hash: e8cbd16037ce
  - coordinate: ProductConnection.filter_facets
    hash: c0320b6e651a
  - coordinate: ProductConnection.filters
    hash: 7c628500f265
  - coordinate: ProductConnection.items
    hash: 453c377929c9
  - coordinate: ProductConnection.pageInfo
    hash: c11711291823
  - coordinate: ProductConnection.range_facets
    hash: bc8f9ad2f5b8
  - coordinate: ProductConnection.term_facets
    hash: 7b98fb273c83
  - coordinate: ProductConnection.totalCount
    hash: d42ece33d253
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# ProductConnection

A GraphQL object type on this deployment's schema, carrying 8 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `edges` | `[ProductEdge]` | A list of all of the edges returned in the connection. |
| `filter_facets` | `[FilterFacet!]!` | Filter facets |
| `filters` | `[SearchProductFilterResult!]` | Parsed filters |
| `items` | `[Product]` | A list of all of the objects returned in the connection. This is a convenience field provided for quickly exploring the API; rather than querying for "{ edges { node } }" when no edge data is needed, this field can be used instead. Note that when clients like Relay need to fetch the "cursor" field on the edge to enable efficient pagination, this shortcut cannot be used, and the full "{ edges { node } } " version should be used instead. |
| `pageInfo` | `PageInfo!` | Information to aid in pagination. |
| `range_facets` | `[RangeFacet!]!` | Range facets |
| `term_facets` | `[TermFacet!]!` | Term facets |
| `totalCount` | `Int` | A count of the total number of objects in this connection, ignoring pagination. This allows a client to fetch the first five objects by passing "5" as the argument to `first`, then fetch the total count so it could display "5 of 83", for example. In cases where we employ infinite scrolling or don't have an exact count of entries, this field will return `null`. |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-productconnection.json`.
