---
id: KB-EBF47FCB
subject: "With design soft (vc-frontend PR 2600) the disabled Email input on /account/profile silently clips a long address at 375 and 1024 px: Inter widens the text past the box and the field shows no ellipsis and cannot be scrolled; classic Lato fits"
plane: experiential
question: Does the disabled email field on /account/profile fit a long address under the soft design?
questions:
  - text: Is the /account/profile email clipped with the soft Inter font?
  - text: Does the soft design cause text overflow on the profile page?
  - text: Why is the account profile email cut off on narrow screens with design soft?
status: active
appliesTo:
  - axis: design
    value: soft
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/profile
evidence:
  - method: observation
    deployment: vcst_qa
    build: e9956932af8d8459
    conditions: theme=vc-frontend PR 2600 @ a4f29534 (local build); setting:design=soft; viewport=375x812 and 1024
    defect: suspected
    at: 2026-11-02T11:21:26.067Z
    by: session:be9f7d04
    who: Lenajava1
---
Local production build of vc-frontend PR 2600 at a4f29534 with design soft, vcst backend, Edge/Chromium, signed in as an existing B2B user whose email is 42 characters long. On /account/profile at 375x812 and at 1024 wide, the disabled Email input's scrollWidth is 369 px inside a 306 px box. The overflow is clipped with no text-overflow ellipsis, and the field is disabled so it cannot be focused or scrolled: the address renders cut mid-domain. The same account on the classic base build (dev 4ce61d0c, Lato) fits the box. This violates BL-UI-004 (silent clip) and is caused by the soft font (Inter is wider than Lato), not by any shape token.
