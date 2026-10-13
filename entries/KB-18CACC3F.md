---
id: KB-18CACC3F
subject: Account types are the localizable dictionary setting VirtoCommerce.Platform.Security.AccountTypes; adding a value saves immediately via POST localizable-settings dictionary-items and becomes assignable as userType
plane: experiential
question: How is a new account (user) type added in Admin and how is it stored and assigned?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/localizable-settings/VirtoCommerce.Platform.Security.AccountTypes/dictionary-items
  - coordinate: DELETE /api/platform/localizable-settings/VirtoCommerce.Platform.Security.AccountTypes/dictionary-items
  - coordinate: GET /api/platform/settings/VirtoCommerce.Platform.Security.AccountTypes
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T16:42:56.344Z
    by: session:624d35a1
    who: kutasinaelena
---
On Stable 16 the Admin path is Settings > Platform > Security > Account types (pencil) - there is no 'User types' section. Add opens 'Edit dictionary value' (System value + one field per locale); Save posts POST /api/platform/localizable-settings/VirtoCommerce.Platform.Security.AccountTypes/dictionary-items -> 204 immediately (no outer Settings Save). Delete is DELETE .../dictionary-items?values=<value> -> 200 after a 'delete selected dictionary values?' Yes/No confirm. The value appears in GET /api/platform/settings/VirtoCommerce.Platform.Security.AccountTypes allowedValues, the Security > Users > New account 'Account type' dropdown shows the en-US label, and a user created with it stores userType = the system value. GET /api/platform/security/roles is 405 and roles are unrelated to account types.
