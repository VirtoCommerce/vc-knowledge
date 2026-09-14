---
id: KB-FE9CD3C7
subject: gql-type-vendor
plane: derived-first
question: What fields does the GraphQL type `Vendor` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: Vendor
    hash: 699e18ad6842
  - coordinate: Vendor.about
    hash: 4eea246875b6
  - coordinate: Vendor.addresses
    hash: fab2977a40f3
  - coordinate: Vendor.defaultBillingAddress
    hash: 982822f0db7f
  - coordinate: Vendor.defaultShippingAddress
    hash: 6f66f12f1e85
  - coordinate: Vendor.dynamicProperties
    hash: 62e4e94671db
  - coordinate: Vendor.emails
    hash: c364d6bf8237
  - coordinate: Vendor.groups
    hash: f69ecc9136e1
  - coordinate: Vendor.iconUrl
    hash: 08730df8eefe
  - coordinate: Vendor.id
    hash: 23d182c998e6
  - coordinate: Vendor.memberType
    hash: 3aa651334d3e
  - coordinate: Vendor.name
    hash: 89b70478ea39
  - coordinate: Vendor.outerId
    hash: 23c767ad21dd
  - coordinate: Vendor.phones
    hash: 99fb84be0da7
  - coordinate: Vendor.rating
    hash: a7e143d73695
  - coordinate: Vendor.seoInfo
    hash: 911253c8c1dc
  - coordinate: Vendor.seoObjectType
    hash: c1a433557f30
  - coordinate: Vendor.siteUrl
    hash: 6f4d26ebc0cf
  - coordinate: Vendor.status
    hash: 206caa392af0
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# Vendor

A GraphQL object type on this deployment's schema, carrying 18 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `about` | `String` | About vendor |
| `addresses` | `MemberAddressConnection` | — |
| `defaultBillingAddress` | `MemberAddressType` | Default billing address |
| `defaultShippingAddress` | `MemberAddressType` | Default shipping address |
| `dynamicProperties` | `[DynamicPropertyValueType]!` | Dynamic property values |
| `emails` | `[String]!` | Emails |
| `groups` | `[String]!` | — |
| `iconUrl` | `String` | Icon URL |
| `id` | `String!` | — |
| `memberType` | `String!` | Member type |
| `name` | `String` | Name |
| `outerId` | `String` | Outer ID |
| `phones` | `[String]!` | Phones |
| `rating` | `Rating` | Vendor rating |
| `seoInfo` | `SeoInfo` | Request related SEO info |
| `seoObjectType` | `String!` | SEO object type |
| `siteUrl` | `String` | Site URL |
| `status` | `String` | Status |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-vendor.json`.
