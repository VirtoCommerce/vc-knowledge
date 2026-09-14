---
id: KB-24DAB8C6
subject: gql-type-inputrequestregistrationtype
plane: derived-first
question: What fields does the GraphQL type `InputRequestRegistrationType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputRequestRegistrationType
    hash: 4959097fdfba
  - coordinate: InputRequestRegistrationType.account
    hash: 9120c691b440
  - coordinate: InputRequestRegistrationType.contact
    hash: c46e7bbe7aba
  - coordinate: InputRequestRegistrationType.languageCode
    hash: f8a4816f660d
  - coordinate: InputRequestRegistrationType.organization
    hash: f63fed9e7aa7
  - coordinate: InputRequestRegistrationType.storeId
    hash: f750fb11945a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputRequestRegistrationType

A GraphQL input object type on this deployment's schema, carrying 5 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `account` | `InputRegisterAccountType!` | Creating contact's account |
| `contact` | `InputRegisterContactType!` | Creating contact |
| `languageCode` | `String` | Notification language code |
| `organization` | `InputRegisterOrganizationType` | company type |
| `storeId` | `String!` | Store ID |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputrequestregistrationtype.json`.
