---
id: KB-D9B2A54F
subject: "The flag-on storefront /designer-preview trusts whatever origin its ep query names: a foreign page that frames it with ep=<own origin>&protocol=2 gets the preview handshake and its forged render message is displayed on the storefront origin"
plane: experiential
question: Can a non-designer page drive the storefront designer preview by naming itself in ?ep=?
questions:
  - text: Does /designer-preview answer connect from a foreign framer?
  - text: "What does a bad or javascript: ep do to the kept designer frame?"
  - text: Is the designer origin kept across a new top-level page?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /designer-preview
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=2.63.0-pr-2676; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store
    defect: suspected
    at: 2026-11-19T18:26:56.147Z
    by: session:51ac2fe3
    who: Lenajava1
---
Theme 2.63.0-pr-2676 (Site Builder flag on). A page on a foreign origin (served locally) framed {storefront}/designer-preview?ep=<that origin>&protocol=2 and posted {type:connect, protocol:2, source:builder}: the frame answered "loaded" to the framer and stored builderPreviewFrame={origin:<foreign>} in sessionStorage; a following {type:render} message with a header, a title and a link to the foreign origin was rendered inside the storefront-origin frame (sanitized HTML; the storefront sends no CSP frame-ancestors). Without ep, no framer got an answer (foreign page, a non-designer page on the platform origin, top-level), and each showed the storefront 404 page; the kept origin did not survive a new top-level document. ep=javascript:alert(1) and ep=not-a-url cleared the kept origin and the frame no longer answered (the expected warning is not visible: the production Logger is a no-op). The same harness against the vcst_dev storefront (theme 2.63.0-alpha.2663) got no reply, but the protocol there may differ, so whether this is new was not settled.
