---
id: KB-BAD0586B
subject: gql-type-inputupdateorderitemdynamicpropertiestype
plane: derived-first
question: What fields does the GraphQL type `InputUpdateOrderItemDynamicPropertiesType` have, and what type is each?
status: active
refutableBy: derivation
appliesTo:
  - surface: graphql
    platformVersion: 3.1007.26
anchors:
  - coordinate: InputUpdateOrderItemDynamicPropertiesType
    hash: 6dff2cc327e6
  - coordinate: InputUpdateOrderItemDynamicPropertiesType.dynamicProperties
    hash: 5a5dbbaa3107
  - coordinate: InputUpdateOrderItemDynamicPropertiesType.lineItemId
    hash: ec3c33d287cb
  - coordinate: InputUpdateOrderItemDynamicPropertiesType.orderId
    hash: 1130b0b703e8
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# InputUpdateOrderItemDynamicPropertiesType

A GraphQL input object type on this deployment's schema, carrying 3 fields. It is what an operation accepts: a required field left out is refused before anything is attempted.

| field | type | what the schema says |
|---|---|---|
| `dynamicProperties` | `[InputDynamicPropertyValueType]!` | — |
| `lineItemId` | `String` | — |
| `orderId` | `String` | — |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/graphql/gql-type-inputupdateorderitemdynamicpropertiestype.json`.
