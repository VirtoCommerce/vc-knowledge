---
id: KB-0ED6FD6E
subject: gql-type-inputupdateapplicationusertype
plane: derived-first
question: What fields does the GraphQL type `InputUpdateApplicationUserType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputUpdateApplicationUserType
    hash: 1ddcc8873f45
  - coordinate: InputUpdateApplicationUserType.accessFailedCount
    hash: f4685b536d8c
  - coordinate: InputUpdateApplicationUserType.email
    hash: 78b70dde6cf2
  - coordinate: InputUpdateApplicationUserType.id
    hash: 23d182c998e6
  - coordinate: InputUpdateApplicationUserType.lockoutEnabled
    hash: ca5485ddc38d
  - coordinate: InputUpdateApplicationUserType.lockoutEnd
    hash: d3236c5fe8a5
  - coordinate: InputUpdateApplicationUserType.memberId
    hash: 421a80f1a210
  - coordinate: InputUpdateApplicationUserType.passwordHash
    hash: b284bd4deab5
  - coordinate: InputUpdateApplicationUserType.phoneNumber
    hash: 337d23eb7171
  - coordinate: InputUpdateApplicationUserType.phoneNumberConfirmed
    hash: 3f4d19f90b58
  - coordinate: InputUpdateApplicationUserType.photoUrl
    hash: 1668802debc3
  - coordinate: InputUpdateApplicationUserType.roles
    hash: 8bc482da13cd
  - coordinate: InputUpdateApplicationUserType.securityStamp
    hash: ab214477eb4a
  - coordinate: InputUpdateApplicationUserType.storeId
    hash: 91cb1fda3072
  - coordinate: InputUpdateApplicationUserType.twoFactorEnabled
    hash: 5c38dacaf3ea
  - coordinate: InputUpdateApplicationUserType.userName
    hash: 2e3e46d47824
  - coordinate: InputUpdateApplicationUserType.userType
    hash: b6c4d63446c4
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputUpdateApplicationUserType

A GraphQL input object type on this deployment's schema, carrying 16 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `accessFailedCount` | `Int` | Failed login attempts for the current user |
| `email` | `String!` | User Email |
| `id` | `String!` | User ID |
| `lockoutEnabled` | `Boolean` | Can user be locked out |
| `lockoutEnd` | `DateTime` | End date of lockout |
| `memberId` | `String` | Id of the associated Member |
| `passwordHash` | `String` | Password Hash |
| `phoneNumber` | `String` | User phone number |
| `phoneNumberConfirmed` | `Boolean` | Is user phone number confirmed |
| `photoUrl` | `String` | User photo URL |
| `roles` | `[InputAssignRoleType]` | List of user roles |
| `securityStamp` | `String!` | SecurityStamp |
| `storeId` | `String` | Associated Store Id |
| `twoFactorEnabled` | `Boolean` | Is Two Factor Authentication enabled |
| `userName` | `String!` | User name |
| `userType` | `String!` | User type (Manager, Customer) |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputupdateapplicationusertype.json`.
