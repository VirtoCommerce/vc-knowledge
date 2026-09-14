---
id: KB-DC8D53F3
subject: gql-type-fulfillmentcenteraddresstype
plane: derived-first
question: What fields does the GraphQL type `FulfillmentCenterAddressType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: FulfillmentCenterAddressType
    hash: 05906b262a2a
  - coordinate: FulfillmentCenterAddressType.addressType
    hash: 0de4b6387c4f
  - coordinate: FulfillmentCenterAddressType.city
    hash: a98e5d293520
  - coordinate: FulfillmentCenterAddressType.countryCode
    hash: 3955a3f4b718
  - coordinate: FulfillmentCenterAddressType.countryName
    hash: 2c0653904022
  - coordinate: FulfillmentCenterAddressType.email
    hash: 0b5c3a2474dd
  - coordinate: FulfillmentCenterAddressType.firstName
    hash: 5f8b5339ae23
  - coordinate: FulfillmentCenterAddressType.id
    hash: 0518e3c59493
  - coordinate: FulfillmentCenterAddressType.key
    hash: 854fec5e8661
  - coordinate: FulfillmentCenterAddressType.lastName
    hash: 58d99298badc
  - coordinate: FulfillmentCenterAddressType.line1
    hash: 7b162b8165b7
  - coordinate: FulfillmentCenterAddressType.line2
    hash: 3d9aa204cc3d
  - coordinate: FulfillmentCenterAddressType.middleName
    hash: 25f2140c236d
  - coordinate: FulfillmentCenterAddressType.name
    hash: 89b70478ea39
  - coordinate: FulfillmentCenterAddressType.organization
    hash: 35d707b8e30f
  - coordinate: FulfillmentCenterAddressType.outerId
    hash: 23c767ad21dd
  - coordinate: FulfillmentCenterAddressType.phone
    hash: 0035e93c954c
  - coordinate: FulfillmentCenterAddressType.postalCode
    hash: 3c0ad5fc7920
  - coordinate: FulfillmentCenterAddressType.regionId
    hash: 79095a589da6
  - coordinate: FulfillmentCenterAddressType.regionName
    hash: 11ef797bdd74
  - coordinate: FulfillmentCenterAddressType.zip
    hash: 77aa137791e1
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# FulfillmentCenterAddressType

A GraphQL object type on this deployment's schema, carrying 20 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `addressType` | `Int` | Address type |
| `city` | `String` | City |
| `countryCode` | `String` | Country code |
| `countryName` | `String` | Country name |
| `email` | `String` | Email |
| `firstName` | `String` | First name |
| `id` | `String` | Id |
| `key` | `String` | Id |
| `lastName` | `String` | Last name |
| `line1` | `String` | Line1 |
| `line2` | `String` | Line2 |
| `middleName` | `String` | Middle name |
| `name` | `String` | Name |
| `organization` | `String` | Company name |
| `outerId` | `String` | Outer id |
| `phone` | `String` | Phone |
| `postalCode` | `String` | Postal code |
| `regionId` | `String` | Region id |
| `regionName` | `String` | Region name |
| `zip` | `String` | Zip |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-fulfillmentcenteraddresstype.json`.
