---
id: KB-2D5BE2F9
subject: gql-type-inputorderaddresstype
plane: derived-first
question: What fields does the GraphQL type `InputOrderAddressType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputOrderAddressType
    hash: 0271048b5062
  - coordinate: InputOrderAddressType.addressType
    hash: 0de4b6387c4f
  - coordinate: InputOrderAddressType.city
    hash: b8923d3e041c
  - coordinate: InputOrderAddressType.countryCode
    hash: 073cf4fb4855
  - coordinate: InputOrderAddressType.countryName
    hash: 2676921e4b50
  - coordinate: InputOrderAddressType.email
    hash: 889467b0fe0c
  - coordinate: InputOrderAddressType.firstName
    hash: 47c86ada379c
  - coordinate: InputOrderAddressType.id
    hash: 7a5a1480e399
  - coordinate: InputOrderAddressType.key
    hash: 8d9122fcf6ac
  - coordinate: InputOrderAddressType.lastName
    hash: 59ed667fcb59
  - coordinate: InputOrderAddressType.line1
    hash: 356872aead60
  - coordinate: InputOrderAddressType.line2
    hash: 711d22350577
  - coordinate: InputOrderAddressType.middleName
    hash: 028ca8f9e6b6
  - coordinate: InputOrderAddressType.name
    hash: c269a0953b20
  - coordinate: InputOrderAddressType.organization
    hash: 05904871611a
  - coordinate: InputOrderAddressType.outerId
    hash: 573f8ad76601
  - coordinate: InputOrderAddressType.phone
    hash: 32cf1b651789
  - coordinate: InputOrderAddressType.postalCode
    hash: e8cf53da7d6b
  - coordinate: InputOrderAddressType.regionId
    hash: 9749196a2a34
  - coordinate: InputOrderAddressType.regionName
    hash: ef68d9bf6b4f
  - coordinate: InputOrderAddressType.zip
    hash: bacc9313d826
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputOrderAddressType

A GraphQL input object type on this deployment's schema, carrying 20 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `addressType` | `Int` | — |
| `city` | `OptionalString` | City |
| `countryCode` | `OptionalString` | Country code |
| `countryName` | `OptionalString` | Country name |
| `email` | `OptionalString` | Email |
| `firstName` | `OptionalString` | First name |
| `id` | `OptionalString` | ID |
| `key` | `OptionalString` | Id |
| `lastName` | `OptionalString` | Last name |
| `line1` | `OptionalString` | Line1 |
| `line2` | `OptionalString` | Line2 |
| `middleName` | `OptionalString` | Middle name |
| `name` | `OptionalString` | Name |
| `organization` | `OptionalString` | Company name |
| `outerId` | `OptionalString` | Outer id |
| `phone` | `OptionalString` | Phone |
| `postalCode` | `OptionalString` | Postal code |
| `regionId` | `OptionalString` | Region id |
| `regionName` | `OptionalString` | Region name |
| `zip` | `OptionalString` | Zip |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputorderaddresstype.json`.
