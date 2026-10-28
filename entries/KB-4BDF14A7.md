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
---
As a signed-in B2B user, /account/change-password renders Current password, New password, Confirm new password, the "Tips for creating a strong password" box and a Change button. No informational notice about being signed out of other devices/browsers is present anywhere on the page (DOM search for 'other devices', 'sessions', 'sign you out' found nothing). Footer build Ver. 2.61.0-alpha.2605.
