---
id: KB-D99D8A0A
subject: Theme 2.51.3 storefront sanitizes injected Page Builder section content on file and database pages the same way 2.59.1 does
plane: experiential
question: Does theme 2.51.3 (Stable 15) render injected unsafe Page Builder section content after the VCST-6570 hotfix?
questions:
  - text: Is the VCST-6570 storefront sanitization present on the 2.51.x theme line?
  - text: "Do javascript: links in Page Builder sections become clickable on theme 2.51.3?"
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /api/pagebuilder/save
  - coordinate: POST /api/page-builder-pages/grouped/{id}/content
  - coordinate: POST /api/pagebuilder/git/publish
evidence:
  - method: observation
    deployment: vcptcore_stable
    build: 3d715345b37f50ec
    conditions: platform=3.1039.19; module:VirtoCommerce.PageBuilderModule=3.1012.4; theme=2.51.3
    at: 2026-11-17T11:56:57.972Z
    by: session:a434dd08
    who: kutasinaelena
    note: "distinct: KB-A4E915A7 is theme 2.59.1 on Stable 16; this is the separately ported 2.51.3 on Stable 15."
---
A file page (POST /api/pagebuilder/save draft=true, published from the Content page blade, which calls POST /api/pagebuilder/git/publish on Page Builder 3.1012.4) and a database page (POST /api/page-builder-pages/grouped/{id}/content, published from the Page Builder shell, which calls /grouped/publishing/{id}?publish=true) each got the VCST-6570 test-plan sections: unknown-section, title with heading "script", text with iframe + img onerror + a href=javascript: + a href=sms: + undeclared data-qa, call-to-action with javascript: and mailto: buttons, slider with a javascript: slide url. On theme 2.51.3 both pages rendered identically: no element for the unknown section; title as h2; no iframe; img src=x with no onerror; javascript: link as <a> with no href; javascript: CTA button as <button> with no href; slide image not wrapped in an anchor; sms: and mailto: stay working links; no data-qa attribute. Clicking each unsafe element logged nothing; console had no SyntaxError. Every rendered section root carries model="[object Object]" and settings="[object Object]" attributes, as on 2.59.1. A safe slide url (/catalog) still renders as an anchor, and https, relative, mailto, tel and sms links in text and CTA buttons keep their href.
