---
id: KB-F4ACB4E5
subject: PUT /api/platform/security/users persists accessFailedCount=0 (it is not one of the protected fields the PUT restores)
plane: experiential
question: Can PUT /api/platform/security/users clear a user's accessFailedCount?
questions:
  - text: How do I clear the failed sign-in counter of a platform user without a successful login?
  - text: Is accessFailedCount one of the protected user fields the user PUT ignores?
concepts:
  - id: account-lockout
  - id: security-account
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/platform/security/users
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 4f722dbd4aaac778
    at: 2026-11-28T20:19:23.576Z
    by: session:c9173e32
    who: kutasinaelena
    note: "distinct: KB-87EC0A28 is about fields the PUT silently ignores; this is the field it does persist (accessFailedCount), which is the opposite outcome on a field that entry does not list."
---
A Manager account read accessFailedCount=4 (lockoutEnabled=true, no lockoutEnd). After an admin resetpassword {forcePasswordChangeOnNextSignIn:false}, a GET of the user followed by a full-entity PUT /api/platform/security/users with accessFailedCount=0 re-read as accessFailedCount=0 and passwordExpired=false. Unlike lockoutEnabled / lockoutEnd / passwordExpired (restored from the database by that PUT, KB-87EC0A28), the failed counter is writable there. Not measured: whether resetpassword by itself already clears the counter (the counter was not read between the two calls).
