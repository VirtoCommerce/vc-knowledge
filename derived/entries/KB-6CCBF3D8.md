---
id: KB-6CCBF3D8
subject: gql-type-inputchangecartconfigureditemtype
plane: derived-first
question: What fields does the GraphQL type `InputChangeCartConfiguredItemType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputChangeCartConfiguredItemType
    hash: 101abd7666d2
  - coordinate: InputChangeCartConfiguredItemType.cartId
    hash: f59bee92dfee
  - coordinate: InputChangeCartConfiguredItemType.cartName
    hash: a2693aff2a50
  - coordinate: InputChangeCartConfiguredItemType.cartType
    hash: 329d90701ee6
  - coordinate: InputChangeCartConfiguredItemType.configurationSections
    hash: b1822048fe63
  - coordinate: InputChangeCartConfiguredItemType.cultureName
    hash: fc312ccec25e
  - coordinate: InputChangeCartConfiguredItemType.currencyCode
    hash: ca3e3446f113
  - coordinate: InputChangeCartConfiguredItemType.lineItemId
    hash: 9d009c9131e0
  - coordinate: InputChangeCartConfiguredItemType.quantity
    hash: deb31a9dba32
  - coordinate: InputChangeCartConfiguredItemType.storeId
    hash: f750fb11945a
  - coordinate: InputChangeCartConfiguredItemType.userId
    hash: e17ca462bb7a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputChangeCartConfiguredItemType

A GraphQL input object type on this deployment's schema, carrying 10 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `cartId` | `String` | — |
| `cartName` | `String` | — |
| `cartType` | `String` | — |
| `configurationSections` | `[ConfigurationSectionInput]` | Configuration sections |
| `cultureName` | `String` | — |
| `currencyCode` | `String` | — |
| `lineItemId` | `String!` | Line item Id |
| `quantity` | `Int` | Quantity |
| `storeId` | `String!` | — |
| `userId` | `String!` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputchangecartconfigureditemtype.json`.
