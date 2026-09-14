---
id: KB-F1B4FFCE
subject: gql-type-accountcreationresulttype
plane: derived-first
question: What fields does the GraphQL type `AccountCreationResultType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: AccountCreationResultType
    hash: aae5dcadcfc1
  - coordinate: AccountCreationResultType.errors
    hash: bd300a73ae0f
  - coordinate: AccountCreationResultType.requireEmailVerification
    hash: a76eaf776ed8
  - coordinate: AccountCreationResultType.succeeded
    hash: 44d48d8c2a77
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# AccountCreationResultType

A GraphQL object type on this deployment's schema, carrying 3 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `errors` | `[RegistrationErrorType]` | The errors that occurred during the operation. |
| `requireEmailVerification` | `Boolean!` | — |
| `succeeded` | `Boolean!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-accountcreationresulttype.json`.
