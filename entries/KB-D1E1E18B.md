---
id: KB-D1E1E18B
subject: On vcptcore-stable the storefront sends no Content-Security-Policy, so a foreign origin can frame /designer-preview; the admin designer preview is refused (admin sends COEP credentialless)
plane: experiential
question: What frame-ancestors policy does the storefront /designer-preview send on vcptcore-stable, and can a foreign page frame it?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /designer-preview
evidence:
  - method: observation
    deployment: vcptcore_stable
    build: 3d715345b37f50ec
    conditions: platform=3.1039.19; module:VirtoCommerce.PageBuilderModule=3.1012.4; theme=2.51.3
    at: 2026-11-17T11:57:11.124Z
    by: session:a434dd08
    who: kutasinaelena
    note: "distinct: KB-50256DE1 is vcptcore_regression; this is the separate Stable 15 stand."
---
GET /designer-preview?ep=..., / and a Page Builder page on the stable storefront answer 200 with only cross-origin-embedder-policy: unsafe-none and cross-origin-resource-policy: cross-origin; no Content-Security-Policy, no X-Frame-Options, no CSP meta in index.html. A page served from http://127.0.0.1 framing /designer-preview?ep=https://example.com loads it (200, app bundle fetched, blank frame waiting for designer messages). The admin host sends X-Frame-Options DENY, CSP frame-ancestors 'self' and COEP credentialless, and the Page Builder designer's own preview iframe shows "refused to connect". Opened standalone, /designer-preview renders the plain storefront 404 page with no designer handshake.
