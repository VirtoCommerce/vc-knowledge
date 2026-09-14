---
id: KB-917C0ADC
subject: gql-type-ordershipmentpackagetype
plane: derived-first
question: What fields does the GraphQL type `OrderShipmentPackageType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: OrderShipmentPackageType
    hash: 68ea0817189a
  - coordinate: OrderShipmentPackageType.barCode
    hash: b2a7905fa8ad
  - coordinate: OrderShipmentPackageType.height
    hash: a461f352da5a
  - coordinate: OrderShipmentPackageType.id
    hash: 23d182c998e6
  - coordinate: OrderShipmentPackageType.items
    hash: be7c2dfc3c87
  - coordinate: OrderShipmentPackageType.length
    hash: 633b8be2c403
  - coordinate: OrderShipmentPackageType.measureUnit
    hash: ac3538a73b60
  - coordinate: OrderShipmentPackageType.packageType
    hash: e6a422ed5136
  - coordinate: OrderShipmentPackageType.weight
    hash: a3098cd3813f
  - coordinate: OrderShipmentPackageType.weightUnit
    hash: bbfe94560b86
  - coordinate: OrderShipmentPackageType.width
    hash: 6d9ad55edc43
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# OrderShipmentPackageType

A GraphQL object type on this deployment's schema, carrying 10 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `barCode` | `String` | — |
| `height` | `Decimal` | — |
| `id` | `String!` | — |
| `items` | `[OrderShipmentItemType!]!` | — |
| `length` | `Decimal` | — |
| `measureUnit` | `String` | — |
| `packageType` | `String` | — |
| `weight` | `Decimal` | — |
| `weightUnit` | `String` | — |
| `width` | `Decimal` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-ordershipmentpackagetype.json`.
