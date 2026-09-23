---
id: KB-D54F3AAB
subject: On virtostart the seeded configurable-product fixtures CFG_LAPTOP and CFG_RING do not exist — both /products-with-options/cfg-parents/* URLs return the storefront 404 page.
plane: experiential
question: Do the @td(CFG_LAPTOP.url) / @td(CFG_RING.url) configurable-product URLs resolve on the virtostart demo storefront?
status: active
appliesTo:
  - axis: concern
    value: test-data-availability
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /products-with-options/cfg-parents/agent-test-config-laptop
  - coordinate: /products-with-options/cfg-parents/agent-test-ring-txt-cfg
evidence:
  - method: observation
    deployment: virtostart
    at: 2026-09-23T10:12:34.194Z
    by: session:cdb27d99
    who: Dan-BV
---
Navigated as a signed-in buyer to /products-with-options/cfg-parents/agent-test-config-laptop and /products-with-options/cfg-parents/agent-test-ring-txt-cfg on virtostart (storefront Ver. 2.58.0). Both render the storefront's 404 page: document title 'Demo - 404 Page not found', h1 '404', h2 'Page not found', with a 'Home page' link. The alias registry test-data/aliases.virtostart.json carries no CFG_LAPTOP or CFG_RING entry either, so this is missing seed data on this stand, not a routing defect. The CAPABILITY is present on virtostart: the demo catalog has a 'Products with options' category (6 products) and its own configurable products render a 'Customize' CTA with a From-price ('Vintage Wedding cake' From $95.00, 'Off-Road Bike. Configurable product' From $200.00 and From $550.00). Consequence for testing: any case bound to the CFG_LAPTOP/CFG_RING aliases is BLOCKED on data on this deployment and must not be re-pointed at a demo product without an alias change, because the fixtures encode specific section shapes (CFG_LAPTOP = 2 required Product sections RAM+Storage with defaults pre-filled; CFG_RING = 1 required Text section maxLength 30 with no default, which is what makes the disabled-until-filled gate observable).
