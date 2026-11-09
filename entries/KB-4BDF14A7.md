---
id: KB-4BDF14A7
subject: The storefront /account/change-password form shows no warning that changing the password signs the user out of other sessions
plane: experiential
question: Does the storefront Change password page warn that other sessions will be signed out?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/change-password
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605
    defect: suspected
    at: 2026-10-28T11:06:08.796Z
    by: session:7e8ea7fb
    who: Lenajava1
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: theme=2.59.0; store=B2B-store
    at: 2026-11-04T21:31:55.650Z
    by: session:35ec50d2
    bodyHash: 3c37fcb06d97
    who: Aleksandra-Mitricheva
    note: "2026-11-05 on theme 2.59.0: same page shape (three password fields, strong-password tips, disabled Change button), no notice about other devices or sessions."
  - method: observation
    deployment: vcptcore_regression
    build: f82810f258ab17b3
    at: 2026-11-09T16:14:42.742Z
    by: session:cb5a6bb9
    bodyHash: 3c37fcb06d97
    who: Aleksandra-Mitricheva
    note: "2026-11-09 vcptcore_regression theme 2.59.0, B2B user: same page, no sign-out notice (regression run REG-2026-11-09-1635-W4, AUTH-069)."
---
As a signed-in B2B user, /account/change-password renders Current password, New password, Confirm new password, the "Tips for creating a strong password" box and a Change button. No informational notice about being signed out of other devices/browsers is present anywhere on the page (DOM search for 'other devices', 'sessions', 'sign you out' found nothing). Footer build Ver. 2.61.0-alpha.2605.
