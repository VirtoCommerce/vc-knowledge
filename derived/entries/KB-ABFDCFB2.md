---
id: KB-ABFDCFB2
subject: gql-type-inputaddwishlistitemstype
plane: derived-first
question: What fields does the GraphQL type `InputAddWishlistItemsType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputAddWishlistItemsType
    hash: 774e8c9f9027
  - coordinate: InputAddWishlistItemsType.listId
    hash: c488b4191149
  - coordinate: InputAddWishlistItemsType.listItems
    hash: 54b9862fafaa
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputAddWishlistItemsType

A GraphQL input object type on this deployment's schema, carrying 2 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `listId` | `String!` | — |
| `listItems` | `[InputNewWishlistItemType!]!` | List items |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputaddwishlistitemstype.json`.
