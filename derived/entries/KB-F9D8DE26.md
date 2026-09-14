---
id: KB-F9D8DE26
subject: gql-type-availabilitydata
plane: derived-first
question: What fields does the GraphQL type `AvailabilityData` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: AvailabilityData
    hash: babc3efd6821
  - coordinate: AvailabilityData.availableQuantity
    hash: 15d44585fab5
  - coordinate: AvailabilityData.inventories
    hash: 79d69bf2a07e
  - coordinate: AvailabilityData.isActive
    hash: 507a118a63bd
  - coordinate: AvailabilityData.isAvailable
    hash: e9bbc872c8f7
  - coordinate: AvailabilityData.isBuyable
    hash: 34da20fc46a0
  - coordinate: AvailabilityData.isEstimated
    hash: fdc0c7307302
  - coordinate: AvailabilityData.isInStock
    hash: 9075cc34dad2
  - coordinate: AvailabilityData.isTrackInventory
    hash: 5b1ef1e11c3a
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# AvailabilityData

A GraphQL object type on this deployment's schema, carrying 8 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `availableQuantity` | `Long!` | Available quantity |
| `inventories` | `[InventoryInfo!]!` | Inventories |
| `isActive` | `Boolean!` | Is active |
| `isAvailable` | `Boolean!` | Is available |
| `isBuyable` | `Boolean!` | Is buyable |
| `isEstimated` | `Boolean!` | Is estimated |
| `isInStock` | `Boolean!` | Is in stock |
| `isTrackInventory` | `Boolean!` | Is track inventory |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-availabilitydata.json`.
