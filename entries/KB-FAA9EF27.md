---
id: KB-FAA9EF27
subject: In print media the storefront keeps the active design's fonts and radius tokens (soft prints Inter/Geologica with 16px containers), and classic print of /cart and order details is unchanged by vc-frontend PR 2600
plane: experiential
question: Does printing /cart or an order details page use the design's typography and shape tokens, and did PR 2600 change classic print?
questions:
  - text: What fonts does the storefront print in with design soft?
  - text: Is the print view of the cart the same on classic before and after the token refactor?
  - text: Does the print stylesheet override --vc-radius or the font tokens?
status: active
appliesTo:
  - axis: media
    value: print
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: vc-frontend
anchors:
  - coordinate: /cart
  - coordinate: /account/orders/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: e9956932af8d8459
    conditions: theme=vc-frontend PR 2600 @ a4f29534 (vcst classic + local soft build); comparison=local classic-BASE dev@4ce61d0c; browser=chromium 1920x1080; media=print (emulated); auth=signed-in buyer
    at: 2026-11-02T09:46:51.615Z
    by: session:be9f7d04
    who: Lenajava1
    note: "distinct: KB-6E8D796E is about how the soft font stylesheet loads on screen; this entry is about print media: whether the print layer keeps or resets the design tokens, and classic print parity before/after the PR."
---
Chromium 1920, signed-in buyer, light, emulated print media, same vcst backend. Fingerprint of every visible element (leaf text: family/size/line-height/weight/letter-spacing/style/case; boxes: radius, shadow). /cart: vcst classic-PR (theme 2.62.0-pr-2600 @ a4f29534) and local classic-BASE dev@4ce61d0c are identical (447 visible, 81 text leaves, 51x8px, 33x4px, 16x3px radii, all Lato, same size/weight/case counts). Order details /account/orders/{id}: identical fingerprints (17x8px, 12x4px, 2x3px; Lato); visible element count 203 vs 201, no typographic difference. Local soft build of the same PR: print keeps Inter body, Geologica h1 and 16px containers (17x16px on order details, 51x16px on /cart), the same sizes, weights and literal 4px/3px radii as classic. The print layer therefore neither resets nor re-maps the design tokens.
