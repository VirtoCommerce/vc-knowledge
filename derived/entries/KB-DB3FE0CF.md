---
id: KB-DB3FE0CF
subject: gql-type-inputcreateorderfromcarttype
plane: derived-first
question: What fields does the GraphQL type `InputCreateOrderFromCartType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputCreateOrderFromCartType
    hash: a0a61adca978
  - coordinate: InputCreateOrderFromCartType.cartId
    hash: f59bee92dfee
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputCreateOrderFromCartType

A GraphQL input object type on this deployment's schema, carrying 1 field. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | Cart ID |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputcreateorderfromcarttype.json`.
