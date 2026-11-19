---
id: KB-9BCD8C34
subject: "POST /api/site-builder/designer/save refuses javascript: and vbscript: links (incl. mixed case, leading space, tab, &#106; entity) but stores a data:text/html link, an img onerror and a script tag verbatim"
plane: experiential
question: Which unsafe values does POST /api/site-builder/designer/save reject in page content, and which does it store?
questions:
  - text: "Does the Site Builder save reject data: URLs in page sections?"
  - text: Does the Site Builder module strip script tags on save?
  - text: Does designer/save accept an unknown section type?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder/designer/save
  - coordinate: GET /api/site-builder/template
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; setting:SiteBuilder.Store.GitContentEnabled=false; role=Administrator
    at: 2026-11-19T13:11:36.773Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=Administrator
    at: 2026-11-19T13:55:18.855Z
    by: session:51ac2fe3
    bodyHash: 8e395acac91a
    who: Lenajava1
    note: "Re-observed on B2B-store (a store whose preview loads): javascript: in a slider slide url -> 400 \"holds a javascript: URL\". A data:text/html link was stored (200) only when designer/save was called WITHOUT catalogHash; with catalogHash (what the Vue designer always sends) the same value -> 400 \"links to a data: URL\". The storefront (2.63.0-pr-2676) dropped the stored data: link when rendering."
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=Administrator
    at: 2026-11-19T14:53:18.434Z
    by: session:51ac2fe3
    bodyHash: 8e395acac91a
    defect: suspected
    who: Lenajava1
    note: "Re-observed on a database page of B2B-store, no catalogHash: javascript: slide url -> 400; data:text/html link, img onerror and script text in a plain-string text value, and section type text-block -> 200, read back verbatim from grouped/{id}/content?draft=true."
---
As admin through the Swagger UI, page draft save of a file page (git off) with sections of an invented type text-block: link values JaVaScRiPt:, "  javascript:", java<TAB>script:, &#106;avascript: and vbscript: -> 400 {error: "The content can run script in the storefront: page: 'content[N].link' holds a javascript: URL, which runs script; ..."}, each field listed (twice: once under page:, once under the file path). In the same request data:text/html,..., %6Aavascript: in href, html "<img src=x onerror=alert(1)>" and text "<script>alert(1)</script>" were not listed. Saved alone (data:text/html;base64 link + img onerror + script text) -> 200 {eTag:null, saveId}, and GET /api/site-builder/template draft=true returned all three values verbatim; the unknown section type was accepted too. Save without an eTag was accepted. The anonymous builderPage kept serving the published copy until the draft was discarded.
