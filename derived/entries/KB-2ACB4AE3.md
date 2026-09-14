---
id: KB-2ACB4AE3
subject: gql-type-initializepaymentresulttype
plane: derived-first
question: What fields does the GraphQL type `InitializePaymentResultType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InitializePaymentResultType
    hash: 18ab457f5868
  - coordinate: InitializePaymentResultType.actionHtmlForm
    hash: 213346ae544d
  - coordinate: InitializePaymentResultType.actionRedirectUrl
    hash: 73b4f0318516
  - coordinate: InitializePaymentResultType.errorMessage
    hash: 7da77b5183e9
  - coordinate: InitializePaymentResultType.isSuccess
    hash: 74ea0a9dd524
  - coordinate: InitializePaymentResultType.orderId
    hash: 1130b0b703e8
  - coordinate: InitializePaymentResultType.orderNumber
    hash: 85f49d6db679
  - coordinate: InitializePaymentResultType.paymentActionType
    hash: 869b4f23c105
  - coordinate: InitializePaymentResultType.paymentId
    hash: a3fd29f45b08
  - coordinate: InitializePaymentResultType.paymentMethodCode
    hash: 4ef843c11fa8
  - coordinate: InitializePaymentResultType.publicParameters
    hash: 2ea559c68865
  - coordinate: InitializePaymentResultType.storeId
    hash: 91cb1fda3072
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InitializePaymentResultType

A GraphQL object type on this deployment's schema, carrying 11 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `actionHtmlForm` | `String` | — |
| `actionRedirectUrl` | `String` | — |
| `errorMessage` | `String` | — |
| `isSuccess` | `Boolean!` | — |
| `orderId` | `String` | — |
| `orderNumber` | `String` | — |
| `paymentActionType` | `String` | — |
| `paymentId` | `String` | — |
| `paymentMethodCode` | `String` | — |
| `publicParameters` | `[KeyValueType]` | — |
| `storeId` | `String` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-initializepaymentresulttype.json`.
