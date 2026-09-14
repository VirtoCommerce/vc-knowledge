---
id: KB-5DF5CA15
subject: gql-type-inputnewwishlistitemtype
plane: derived-first
question: What fields does the GraphQL type `InputNewWishlistItemType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputNewWishlistItemType
    hash: 3a18ce00d368
  - coordinate: InputNewWishlistItemType.productId
    hash: bea38fdaba6d
  - coordinate: InputNewWishlistItemType.quantity
    hash: deb31a9dba32
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputNewWishlistItemType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `productId` | `String!` | Product Id |
| `quantity` | `Int` | Product quantity |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputnewwishlistitemtype.json`.
