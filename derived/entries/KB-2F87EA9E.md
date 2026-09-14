---
id: KB-2F87EA9E
subject: gql-type-contacttype
plane: derived-first
question: What fields does the GraphQL type `ContactType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: ContactType
    hash: 996c417d5b78
  - coordinate: ContactType.about
    hash: 183ff5085ee8
  - coordinate: ContactType.addresses
    hash: fab2977a40f3
  - coordinate: ContactType.birthDate
    hash: 9cb92321a3d8
  - coordinate: ContactType.currencyCode
    hash: ca3e3446f113
  - coordinate: ContactType.defaultBillingAddress
    hash: 982822f0db7f
  - coordinate: ContactType.defaultLanguage
    hash: 37bb0b9b46d5
  - coordinate: ContactType.defaultShippingAddress
    hash: 6f66f12f1e85
  - coordinate: ContactType.dynamicProperties
    hash: 62e4e94671db
  - coordinate: ContactType.emails
    hash: c364d6bf8237
  - coordinate: ContactType.firstName
    hash: 70c8a1eb9908
  - coordinate: ContactType.fullName
    hash: 9087588776df
  - coordinate: ContactType.groups
    hash: f69ecc9136e1
  - coordinate: ContactType.id
    hash: 23d182c998e6
  - coordinate: ContactType.lastName
    hash: 6c8bc6fd8b3a
  - coordinate: ContactType.memberType
    hash: 3aa651334d3e
  - coordinate: ContactType.middleName
    hash: 25f2140c236d
  - coordinate: ContactType.name
    hash: 89b70478ea39
  - coordinate: ContactType.organization
    hash: 759ba67bcc47
  - coordinate: ContactType.organizationId
    hash: abd73e10e07c
  - coordinate: ContactType.organizations
    hash: 6bcaba4c7997
  - coordinate: ContactType.organizationsIds
    hash: a6e5b2dd2376
  - coordinate: ContactType.outerId
    hash: 23c767ad21dd
  - coordinate: ContactType.phones
    hash: 99fb84be0da7
  - coordinate: ContactType.securityAccounts
    hash: ab21cae2caff
  - coordinate: ContactType.selectedAddressId
    hash: bc8e127ba2f7
  - coordinate: ContactType.seoInfo
    hash: 911253c8c1dc
  - coordinate: ContactType.seoObjectType
    hash: c1a433557f30
  - coordinate: ContactType.status
    hash: 206caa392af0
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# ContactType

A GraphQL object type on this deployment's schema, carrying 28 fields. It is what an operation returns: a field that is not here cannot be selected, however plausible its name.

| field | type | what the schema says |
|---|---|---|
| `about` | `String!` | — |
| `addresses` | `MemberAddressConnection` | — |
| `birthDate` | `Date` | — |
| `currencyCode` | `String` | — |
| `defaultBillingAddress` | `MemberAddressType` | Default billing address |
| `defaultLanguage` | `String` | — |
| `defaultShippingAddress` | `MemberAddressType` | Default shipping address |
| `dynamicProperties` | `[DynamicPropertyValueType]!` | Dynamic property values |
| `emails` | `[String]!` | Emails |
| `firstName` | `String!` | — |
| `fullName` | `String!` | — |
| `groups` | `[String]!` | — |
| `id` | `String!` | — |
| `lastName` | `String!` | — |
| `memberType` | `String!` | Member type |
| `middleName` | `String` | — |
| `name` | `String` | Name |
| `organization` | `Organization` | — |
| `organizationId` | `String` | — |
| `organizations` | `OrganizationConnection` | — |
| `organizationsIds` | `[String]!` | — |
| `outerId` | `String` | Outer ID |
| `phones` | `[String]!` | Phones |
| `securityAccounts` | `[UserType]` | — |
| `selectedAddressId` | `String` | Selected shipping address id. |
| `seoInfo` | `SeoInfo` | Request related SEO info |
| `seoObjectType` | `String!` | SEO object type |
| `status` | `String` | Status |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-contacttype.json`.
