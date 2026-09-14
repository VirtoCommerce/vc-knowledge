---
id: KB-CEB816BB
subject: gql-mutations-createreview
plane: derived-first
question: What is the signature of the GraphQL mutation `Mutations.createReview`, and what do its inputs require?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    root: Mutations
    platformVersion: 3.1007.26
anchors:
  - coordinate: Mutations.createReview
    hash: 596f1887b3cf
  - coordinate: CreateReviewCommandType
    hash: a2af3cee0266
  - coordinate: CreateReviewResult
    hash: cb33a630555f
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Mutations.createReview

A GraphQL mutation field on the root type `Mutations`. The root type names on this deployment are read from introspection, not assumed: `Query` / `Mutations` / `Subscriptions`.

```graphql
createReview(command: CreateReviewCommandType!): CreateReviewResult
```

| argument | type | required | what the schema says |
|---|---|---|---|
| `command` | `CreateReviewCommandType!` | yes | — |

Types in this signature: `CreateReviewCommandType` (INPUT_OBJECT) — `gql-type-createreviewcommandtype`, `CreateReviewResult` (OBJECT) — `gql-type-createreviewresult`.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-mutations-createreview.json`.
