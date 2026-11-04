---
id: KB-E451FDF9
subject: In a store with MultiFactorAuthentication.StoreMode AdminManaged, requestMfaChangeCode is refused in the payload with errorCode mfa_self_service_not_allowed and no code email is sent
plane: experiential
question: What does the requestMfaChangeCode mutation return for a signed-in customer when the store's MFA StoreMode is AdminManaged?
questions:
  - text: Can a customer turn MFA on over xAPI when the store administrator manages two-factor authentication?
  - text: Is a change code emailed when requestMfaChangeCode is refused in an AdminManaged store?
  - text: Which errorCode does requestMfaChangeCode give when customer self-service MFA is not allowed?
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.requestMfaChangeCode
  - coordinate: POST /graphql
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: module:VirtoCommerce.MFA=3.1002.0-pr-3; theme=2.62.0-pr-2622; setting:MultiFactorAuthentication.StoreMode=AdminManaged; setting:MultiFactorAuthentication.Enabled=false
    at: 2026-11-04T17:54:50.250Z
    by: session:7f21950d
    who: Lenajava1
---
Live on vcst_qa, 2026-11-04 (MFA module 3.1002.0-pr-3, theme 2.62.0-pr-2622, global MultiFactorAuthentication.Enabled=false, store StoreMode=AdminManaged): a confirmed-email customer of the store with user MFA mode Off signed in by password (no challenge), then POST /graphql requestMfaChangeCode(command:{storeId}) with the customer's bearer answered HTTP 200, no errors[], data.requestMfaChangeCode = {succeeded:false, twoFactorToken:null, errorCode:"mfa_self_service_not_allowed", errorDescription:"The store doesn't let customers manage multi-factor authentication."}. The MfaSignInEmailNotification journal for the account stayed at 0 rows (checked ~8 s later): the refusal happens before any code is sent. The refusal is a payload errorCode, not a GraphQL error.
