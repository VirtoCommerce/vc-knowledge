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
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; theme=2.59.0; store=B2B-store
    at: 2026-11-10T04:16:14.152Z
    by: session:3f3bb000
    bodyHash: 3c37fcb06d97
    who: kutasinaelena
    note: "2026-11-10 after env upgrade to platform 3.1076.13, theme 2.59.0, B2B buyer: same page, no notice about other devices/sessions (REG-2026-11-10-0401, SEC-AUTH-005)."
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; store=B2B-store
    at: 2026-11-11T21:35:07.365Z
    by: session:7e403256
    bodyHash: 3c37fcb06d97
    who: kutasinaelena
    note: "2026-11-12 vcst_qa theme 2.62.0-alpha.2647, B2B buyer: same page shape (three fields, tips, disabled Change), no notice about other devices/sessions (REG-2026-11-12-0000-W1, SEC-AUTH-005)."
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: platform=3.1097.0; theme=2.62.0-alpha.2647; store=B2B-store
    at: 2026-11-11T21:55:33.972Z
    by: session:7e403256
    bodyHash: 3c37fcb06d97
    who: kutasinaelena
    note: "REG-2026-11-12-0000-W1 AUTH-069, TECHFLOW buyer: no notice on /account/change-password."
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; store=B2B-store
    at: 2026-11-12T03:38:16.821Z
    by: session:7e403256
    bodyHash: 3c37fcb06d97
    defect: none
    who: kutasinaelena
    note: "Form still shows no pre-submit notice (2026-11-12, B2B buyer). Withdrawing the defect marker: the VCST-4531 AC asks for a GREEN POP-UP after the change (\"User changes password on Device A -> Green pop-up should be displayed: 'Changing your password will sign you out of all other devices and browsers.'\"), not a notice on the form. vc-frontend PR #2275 (2.49.0) put that text into pages.change_password.success_message, which pages/account/change-password.vue shows via notifications.success on submit success. Absence on the form is by design."
---
As a signed-in B2B user, /account/change-password renders Current password, New password, Confirm new password, the "Tips for creating a strong password" box and a Change button. No informational notice about being signed out of other devices/browsers is present anywhere on the page (DOM search for 'other devices', 'sessions', 'sign you out' found nothing). Footer build Ver. 2.61.0-alpha.2605.
