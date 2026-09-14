---
id: KB-A8F954FF
subject: gql-type-productassociation
plane: derived-first
question: What fields does the GraphQL type `ProductAssociation` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: ProductAssociation
    hash: cb8c37921ec0
  - coordinate: ProductAssociation.associatedObjectId
    hash: 7453fbe2cdb8
  - coordinate: ProductAssociation.associatedObjectType
    hash: 71f9307051f3
  - coordinate: ProductAssociation.priority
    hash: 592635496c18
  - coordinate: ProductAssociation.product
    hash: ae13b65e932e
  - coordinate: ProductAssociation.quantity
    hash: deb31a9dba32
  - coordinate: ProductAssociation.tags
    hash: 4ee222597347
  - coordinate: ProductAssociation.type
    hash: fc449b03ed47
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# ProductAssociation

A GraphQL object type on this deployment's schema, carrying 7 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `associatedObjectId` | `String` | — |
| `associatedObjectType` | `String` | — |
| `priority` | `Int!` | — |
| `product` | `Product` | — |
| `quantity` | `Int` | — |
| `tags` | `[String!]!` | — |
| `type` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-productassociation.json`.
