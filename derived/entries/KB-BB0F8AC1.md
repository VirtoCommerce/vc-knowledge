---
id: KB-BB0F8AC1
subject: gql-type-paymenttype
plane: derived-first
question: What fields does the GraphQL type `PaymentType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: PaymentType
    hash: b51138a12531
  - coordinate: PaymentType.amount
    hash: c39319efac35
  - coordinate: PaymentType.billingAddress
    hash: 00095afb9147
  - coordinate: PaymentType.comment
    hash: daa8fcab278e
  - coordinate: PaymentType.currency
    hash: 477d7780102b
  - coordinate: PaymentType.discountAmount
    hash: 720d91abfd25
  - coordinate: PaymentType.discountAmountWithTax
    hash: f286eb49baff
  - coordinate: PaymentType.discounts
    hash: 7470ef6cfe13
  - coordinate: PaymentType.dynamicProperties
    hash: 36dfdf7c65f8
  - coordinate: PaymentType.id
    hash: 23d182c998e6
  - coordinate: PaymentType.outerId
    hash: 23c767ad21dd
  - coordinate: PaymentType.paymentGatewayCode
    hash: 9dfa1b48b2a0
  - coordinate: PaymentType.price
    hash: 87c6a0ffb12e
  - coordinate: PaymentType.priceWithTax
    hash: 8a6922a553ad
  - coordinate: PaymentType.purpose
    hash: d0de6545a058
  - coordinate: PaymentType.taxDetails
    hash: 1fb256193ae6
  - coordinate: PaymentType.taxPercentRate
    hash: 4d66f988b6ce
  - coordinate: PaymentType.taxTotal
    hash: 897a91cd1946
  - coordinate: PaymentType.taxType
    hash: 695603216f5c
  - coordinate: PaymentType.total
    hash: 52359287f058
  - coordinate: PaymentType.totalWithTax
    hash: e46dad44137a
  - coordinate: PaymentType.vendor
    hash: a0c7d3fa8143
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# PaymentType

A GraphQL object type on this deployment's schema, carrying 21 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `amount` | `MoneyType!` | Amount |
| `billingAddress` | `CartAddressType` | Billing address |
| `comment` | `String` | Text comment |
| `currency` | `CurrencyType!` | Currency |
| `discountAmount` | `MoneyType!` | Discount amount |
| `discountAmountWithTax` | `MoneyType!` | Discount amount with tax |
| `discounts` | `[DiscountType]!` | Discounts |
| `dynamicProperties` | `[DynamicPropertyValueType!]!` | Cart payment dynamic property values |
| `id` | `String!` | Payment Id |
| `outerId` | `String` | Value of payment outer id |
| `paymentGatewayCode` | `String` | Value of payment gateway code |
| `price` | `MoneyType!` | Price |
| `priceWithTax` | `MoneyType!` | Price with tax |
| `purpose` | `String` | — |
| `taxDetails` | `[TaxDetailType!]!` | Tax details |
| `taxPercentRate` | `Decimal!` | Tax percent rate |
| `taxTotal` | `MoneyType!` | Tax total |
| `taxType` | `String` | Tax type |
| `total` | `MoneyType!` | Total |
| `totalWithTax` | `MoneyType!` | Total with tax |
| `vendor` | `CommonVendor` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-paymenttype.json`.
