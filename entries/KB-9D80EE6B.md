---
id: KB-9D80EE6B
subject: the Remember me checkbox on /sign-in has no effect on the session
plane: experiential
question: Does ticking Remember me on /sign-in change how long the storefront session lasts?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:15:11.724Z
    by: session:memimpor
    who: Lenajava1
---
The Remember me checkbox on the storefront /sign-in page is vestigial UI with no backend contract. With it on or off the token request is identical (scope offline_access always), the session is persisted the same way in local storage, and the same long-lived refresh token is issued. The feature was never implemented, per the product owner - treat it as a known gap or feature request, not a defect.
