---
id: KB-9D80EE6B
subject: the Remember me checkbox on /sign-in has no effect on the session
plane: experiential
question: Does ticking Remember me on /sign-in change how long the storefront session lasts?
questions:
  - text: If I tick Remember me when signing in, will I stay logged in longer?
  - text: Should QA file a defect because the Remember me checkbox on sign-in changes nothing?
  - text: Does the Remember me option change the token request scope, refresh token or session persistence?
concepts:
  - id: session
  - id: sign-in
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:15:11.724Z
    by: session:memimpor
    who: Lenajava1
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: theme=2.63.0-alpha.2681; platform=3.1101.0
    at: 2026-11-26T00:15:39.860Z
    by: session:779b94fc
    bodyHash: 614d3b7430a5
    who: kutasinaelena
    note: "2026-11-26, theme 2.63.0-alpha.2681: Remember me unchecked by default; with it checked the session survives closing the tab and opening a new one in the same context; with it unchecked a new browser context is signed out - consistent with storage-based persistence independent of the checkbox."
---
The Remember me checkbox on the storefront /sign-in page is vestigial UI with no backend contract. With it on or off the token request is identical (scope offline_access always), the session is persisted the same way in local storage, and the same long-lived refresh token is issued. The feature was never implemented, per the product owner - treat it as a known gap or feature request, not a defect.
