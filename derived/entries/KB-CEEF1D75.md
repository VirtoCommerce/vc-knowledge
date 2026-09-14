---
id: KB-CEEF1D75
subject: gql-type-menulinktype
plane: derived-first
question: What fields does the GraphQL type `MenuLinkType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: MenuLinkType
    hash: de15d659d80a
  - coordinate: MenuLinkType.associatedObjectId
    hash: 7453fbe2cdb8
  - coordinate: MenuLinkType.associatedObjectName
    hash: 4abd3b6ed8e3
  - coordinate: MenuLinkType.associatedObjectType
    hash: 71f9307051f3
  - coordinate: MenuLinkType.childItems
    hash: 7b4a2cc2eb5c
  - coordinate: MenuLinkType.outerId
    hash: 23c767ad21dd
  - coordinate: MenuLinkType.priority
    hash: 592635496c18
  - coordinate: MenuLinkType.title
    hash: 224ca962cda1
  - coordinate: MenuLinkType.url
    hash: a6e93cecb2fe
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# MenuLinkType

A GraphQL object type on this deployment's schema, carrying 8 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `associatedObjectId` | `String` | Menu item object ID |
| `associatedObjectName` | `String` | Menu item object name |
| `associatedObjectType` | `String` | Menu item type name |
| `childItems` | `[MenuLinkType!]!` | — |
| `outerId` | `String` | Menu item outerID |
| `priority` | `Int!` | Menu item priority |
| `title` | `String!` | Menu item title |
| `url` | `String!` | Menu item url |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-menulinktype.json`.
