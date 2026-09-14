---
id: KB-53DF30DC
subject: gql-type-productsuggestionsqueryresponsetype
plane: derived-first
question: What fields does the GraphQL type `ProductSuggestionsQueryResponseType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: ProductSuggestionsQueryResponseType
    hash: a57b0570bef3
  - coordinate: ProductSuggestionsQueryResponseType.suggestions
    hash: cc06c67997cb
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# ProductSuggestionsQueryResponseType

A GraphQL object type on this deployment's schema, carrying 1 field. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `suggestions` | `[String]` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-productsuggestionsqueryresponsetype.json`.
