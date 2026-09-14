---
id: KB-DEEEFD73
subject: gql-type-inputcreatecartfromwishlisttype
plane: derived-first
question: What fields does the GraphQL type `InputCreateCartFromWishlistType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputCreateCartFromWishlistType
    hash: 02b25dc74281
  - coordinate: InputCreateCartFromWishlistType.listId
    hash: c488b4191149
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputCreateCartFromWishlistType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `listId` | `String!` | Wishlist ID |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputcreatecartfromwishlisttype.json`.
