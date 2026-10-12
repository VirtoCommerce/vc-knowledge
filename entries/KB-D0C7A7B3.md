---
id: KB-D0C7A7B3
subject: The storefront's 15 s "incompatible backend" toast collapses its notifications host when it expires, which adds an unrelated layout shift to CLS on account pages
plane: experiential
question: storefront app start shows an "incompatible backend" outdated modules toast for 15 s; does its expiry cause a layout shift that confounds CLS measurements on account pages at 375 px
status: active
appliesTo:
  - axis: engine
    value: chromium
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 375
anchors:
  - coordinate: /account/returns
evidence:
  - method: observation (layout-shift entry sources recorded at document start)
    deployment: vcptcore_dev
    at: 2026-10-10T01:11:17.896Z
    by: session:61a7b05a
    who: yuskithedeveloper
---
On a stand where the storefront's expected module versions are ahead of the deployed ones, every app start shows a red "frontend application expects newer versions of the Platform and one or more Modules" toast for about 15 s (client-app/app-runner.ts:363-368). At 375x812 on /account/returns, when it expires the section.notifications-host collapses (rect [0,490,360,322] to [0,788,360,24]) and Chromium records a layout shift of about 0.1455 with no recent input. A CLS reading taken within ~15 s of app start therefore includes this environment artefact; wait for the toast to expire (or exclude entries whose only source is section.notifications-host) before measuring a page's own layout stability.
