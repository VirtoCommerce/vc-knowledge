---
id: KB-F63E4A4D
subject: gql-type-createreviewresult
plane: derived-first
question: What fields does the GraphQL type `CreateReviewResult` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: CreateReviewResult
    hash: fdfa047221a5
  - coordinate: CreateReviewResult.id
    hash: 0518e3c59493
  - coordinate: CreateReviewResult.userName
    hash: ff43389afba9
  - coordinate: CreateReviewResult.validationErrors
    hash: c74bf6721f10
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# CreateReviewResult

A GraphQL object type on this deployment's schema, carrying 3 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `id` | `String` | — |
| `userName` | `String` | — |
| `validationErrors` | `[ReviewValidationErrorType!]!` | A set of errors in case the review is invalid |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-createreviewresult.json`.
