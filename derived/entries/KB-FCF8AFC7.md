---
id: KB-FCF8AFC7
subject: gql-type-inputremovewishlistitemtype
plane: derived-first
question: What fields does the GraphQL type `InputRemoveWishlistItemType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputRemoveWishlistItemType
    hash: fe33714021ff
  - coordinate: InputRemoveWishlistItemType.lineItemId
    hash: ec3c33d287cb
  - coordinate: InputRemoveWishlistItemType.listId
    hash: c488b4191149
  - coordinate: InputRemoveWishlistItemType.productId
    hash: 8f0b9285ad20
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputRemoveWishlistItemType

A GraphQL input object type on this deployment's schema, carrying 3 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `lineItemId` | `String` | Line item ID to remove |
| `listId` | `String!` | List ID |
| `productId` | `String` | Line item product ID to remove |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputremovewishlistitemtype.json`.
