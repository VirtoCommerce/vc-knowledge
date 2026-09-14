---
id: KB-9A07ABAA
subject: gql-type-inputcreatewishlisttype
plane: derived-first
question: What fields does the GraphQL type `InputCreateWishlistType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputCreateWishlistType
    hash: ccbc7eff7469
  - coordinate: InputCreateWishlistType.cultureName
    hash: fc312ccec25e
  - coordinate: InputCreateWishlistType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputCreateWishlistType.description
    hash: d2a651ddaa3a
  - coordinate: InputCreateWishlistType.listName
    hash: 733a95bad1c2
  - coordinate: InputCreateWishlistType.scope
    hash: 4e179e8259a3
  - coordinate: InputCreateWishlistType.sharingKey
    hash: dc5b40b868ca
  - coordinate: InputCreateWishlistType.storeId
    hash: f750fb11945a
  - coordinate: InputCreateWishlistType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputCreateWishlistType

A GraphQL input object type on this deployment's schema, carrying 8 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cultureName` | `String` | Culture name |
| `currencyCode` | `String` | Currency code |
| `description` | `String` | List description |
| `listName` | `String` | List name |
| `scope` | `String` | List scope (private or organization) |
| `sharingKey` | `String` | Sharing key (URL argument) |
| `storeId` | `String!` | Store ID |
| `userId` | `String!` | Owner ID |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputcreatewishlisttype.json`.
