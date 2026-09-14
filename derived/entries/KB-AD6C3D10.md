---
id: KB-AD6C3D10
subject: gql-type-currencytype
plane: derived-first
question: What fields does the GraphQL type `CurrencyType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: CurrencyType
    hash: 40c3882b2973
  - coordinate: CurrencyType.code
    hash: 67ae70c8a23f
  - coordinate: CurrencyType.cultureName
    hash: 6c981ea9c28a
  - coordinate: CurrencyType.customFormatting
    hash: bb39d005eaa0
  - coordinate: CurrencyType.englishName
    hash: 433280707e2e
  - coordinate: CurrencyType.exchangeRate
    hash: 205418082321
  - coordinate: CurrencyType.symbol
    hash: 285393c10124
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# CurrencyType

A GraphQL object type on this deployment's schema, carrying 6 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `code` | `String!` | Currency code may be used ISO 4217 |
| `cultureName` | `String!` | Currency English name |
| `customFormatting` | `String` | Currency custom formatting |
| `englishName` | `String!` | Currency English name |
| `exchangeRate` | `Decimal!` | Exchange rate |
| `symbol` | `String!` | Symbol |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-currencytype.json`.
