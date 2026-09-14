---
id: KB-EE2EBA0D
subject: gql-type-inputaddwishlistbulkitemtype
plane: derived-first
question: What fields does the GraphQL type `InputAddWishlistBulkItemType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputAddWishlistBulkItemType
    hash: 0cfaae8f25e9
  - coordinate: InputAddWishlistBulkItemType.configurationSections
    hash: b1822048fe63
  - coordinate: InputAddWishlistBulkItemType.listIds
    hash: 781f5938d1bd
  - coordinate: InputAddWishlistBulkItemType.productId
    hash: bea38fdaba6d
  - coordinate: InputAddWishlistBulkItemType.quantity
    hash: deb31a9dba32
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputAddWishlistBulkItemType

A GraphQL input object type on this deployment's schema, carrying 4 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `configurationSections` | `[ConfigurationSectionInput]` | Configurable product support. List of configurable product sections |
| `listIds` | `[String]!` | Wish list ids |
| `productId` | `String!` | Product id to add |
| `quantity` | `Int` | Product quantity to add |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputaddwishlistbulkitemtype.json`.
