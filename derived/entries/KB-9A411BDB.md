---
id: KB-9A411BDB
subject: gql-type-cartconfigurationitemtype
plane: derived-first
question: What fields does the GraphQL type `CartConfigurationItemType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: CartConfigurationItemType
    hash: d3170c7f0121
  - coordinate: CartConfigurationItemType.customText
    hash: ab8bd10f1e53
  - coordinate: CartConfigurationItemType.files
    hash: a798f55735f0
  - coordinate: CartConfigurationItemType.id
    hash: 23d182c998e6
  - coordinate: CartConfigurationItemType.name
    hash: 89b70478ea39
  - coordinate: CartConfigurationItemType.productId
    hash: 8f0b9285ad20
  - coordinate: CartConfigurationItemType.quantity
    hash: deb31a9dba32
  - coordinate: CartConfigurationItemType.sectionId
    hash: 42536b2624d1
  - coordinate: CartConfigurationItemType.type
    hash: fc449b03ed47
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# CartConfigurationItemType

A GraphQL object type on this deployment's schema, carrying 8 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `customText` | `String` | Custom text for 'Text' configuration item section |
| `files` | `[CartConfigurationItemFileType]` | List of files for 'File' configuration item section |
| `id` | `String!` | Configuration item ID |
| `name` | `String` | Configuration item name |
| `productId` | `String` | Configuration item product ID |
| `quantity` | `Int` | Configuration item product quantity |
| `sectionId` | `String!` | Configuration item section ID |
| `type` | `String!` | Configuration item type. Possible values: 'Product', 'Text', 'File' |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-cartconfigurationitemtype.json`.
