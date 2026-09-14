---
id: KB-0DA841CD
subject: gql-type-inputorderpaymenttype
plane: derived-first
question: What fields does the GraphQL type `InputOrderPaymentType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputOrderPaymentType
    hash: b001f7a83316
  - coordinate: InputOrderPaymentType.amount
    hash: 93a94e1e4bac
  - coordinate: InputOrderPaymentType.billingAddress
    hash: 5540d22516ea
  - coordinate: InputOrderPaymentType.comment
    hash: 6c523e302391
  - coordinate: InputOrderPaymentType.currency
    hash: b9edb2adb1e7
  - coordinate: InputOrderPaymentType.dynamicProperties
    hash: f06faa535377
  - coordinate: InputOrderPaymentType.id
    hash: 7a5a1480e399
  - coordinate: InputOrderPaymentType.outerId
    hash: 573f8ad76601
  - coordinate: InputOrderPaymentType.paymentGatewayCode
    hash: dbedceec1964
  - coordinate: InputOrderPaymentType.price
    hash: 1bb57e0be209
  - coordinate: InputOrderPaymentType.vendorId
    hash: b1524d43be43
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputOrderPaymentType

A GraphQL input object type on this deployment's schema, carrying 10 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `amount` | `OptionalDecimal` | — |
| `billingAddress` | `InputOrderAddressType` | — |
| `comment` | `OptionalString` | Text comment |
| `currency` | `OptionalString` | — |
| `dynamicProperties` | `[InputDynamicPropertyValueType]` | Dynamic properties |
| `id` | `OptionalString` | Payment ID |
| `outerId` | `OptionalString` | Payment outer ID value |
| `paymentGatewayCode` | `OptionalString` | Payment gateway code value |
| `price` | `OptionalDecimal` | — |
| `vendorId` | `OptionalString` | Payment vendor ID value |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputorderpaymenttype.json`.
