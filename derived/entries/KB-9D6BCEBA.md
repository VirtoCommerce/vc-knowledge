---
id: KB-9D6BCEBA
subject: gql-type-customerreviewimage
plane: derived-first
question: What fields does the GraphQL type `CustomerReviewImage` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: CustomerReviewImage
    hash: 5bfdbb6a58a8
  - coordinate: CustomerReviewImage.id
    hash: 23d182c998e6
  - coordinate: CustomerReviewImage.name
    hash: d212a4ea663c
  - coordinate: CustomerReviewImage.url
    hash: a6e93cecb2fe
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# CustomerReviewImage

A GraphQL object type on this deployment's schema, carrying 3 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `id` | `String!` | — |
| `name` | `String!` | — |
| `url` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-customerreviewimage.json`.
