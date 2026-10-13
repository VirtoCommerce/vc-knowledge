---
id: KB-87C33CCC
subject: The storefront's entry bundle (/assets/index-<hash>.js, referenced from the index HTML) inlines the deployed theme version as x="<version>",y={version:x}, readable anonymously
plane: experiential
question: Where can the deployed storefront theme (vc-frontend) version be read without a browser or admin token?
status: active
appliesTo:
  - axis: layer
    value: deployment
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /assets/index-*.js
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2543-0127-01276486; platform=3.1078.0
    at: 2026-10-13T12:57:49.368Z
    by: session:8f68ece7
    who: Lenajava1
---
The storefront index HTML (cache-busted with a query string, since it is served max-age=7200 — see KB-D9DF8E54) references one module entry script /assets/index-<hash>.js. That bundle contains the vc-frontend package version inlined exactly once as a string bound to a {version:…} object, e.g. "2.60.0-pr-2543-0127-01276486",nln={version:eln}. It equals the build string of the deployed theme (PR builds included), so it is a live, anonymous source for the theme version; /api/platform/modules does not report the theme. The footer "Ver." shows the same value but is SPA-rendered, so it is not in the raw HTML.
