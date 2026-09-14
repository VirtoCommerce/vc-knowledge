---
id: KB-BF4592ED
subject: gql-type-pickupaddresstype
plane: derived-first
question: What fields does the GraphQL type `PickupAddressType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: PickupAddressType
    hash: 684f9c7e23f8
  - coordinate: PickupAddressType.addressType
    hash: 0de4b6387c4f
  - coordinate: PickupAddressType.city
    hash: a98e5d293520
  - coordinate: PickupAddressType.countryCode
    hash: 3955a3f4b718
  - coordinate: PickupAddressType.countryName
    hash: 2c0653904022
  - coordinate: PickupAddressType.description
    hash: d2a651ddaa3a
  - coordinate: PickupAddressType.email
    hash: 0b5c3a2474dd
  - coordinate: PickupAddressType.id
    hash: 23d182c998e6
  - coordinate: PickupAddressType.key
    hash: 854fec5e8661
  - coordinate: PickupAddressType.line1
    hash: 7b162b8165b7
  - coordinate: PickupAddressType.line2
    hash: 3d9aa204cc3d
  - coordinate: PickupAddressType.name
    hash: 89b70478ea39
  - coordinate: PickupAddressType.organization
    hash: 35d707b8e30f
  - coordinate: PickupAddressType.outerId
    hash: 23c767ad21dd
  - coordinate: PickupAddressType.phone
    hash: 0035e93c954c
  - coordinate: PickupAddressType.postalCode
    hash: 3c0ad5fc7920
  - coordinate: PickupAddressType.regionId
    hash: 79095a589da6
  - coordinate: PickupAddressType.regionName
    hash: 11ef797bdd74
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# PickupAddressType

A GraphQL object type on this deployment's schema, carrying 17 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `addressType` | `Int` | Address type |
| `city` | `String` | City |
| `countryCode` | `String` | Country code |
| `countryName` | `String` | Country name |
| `description` | `String` | Description |
| `email` | `String` | Email |
| `id` | `String!` | Id |
| `key` | `String` | Key |
| `line1` | `String` | Line1 |
| `line2` | `String` | Line2 |
| `name` | `String` | Name |
| `organization` | `String` | Company name |
| `outerId` | `String` | Outer id |
| `phone` | `String` | Phone |
| `postalCode` | `String` | Postal code |
| `regionId` | `String` | Region id |
| `regionName` | `String` | Region name |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-pickupaddresstype.json`.
