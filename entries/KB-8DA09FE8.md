---
id: KB-8DA09FE8
subject: "Push Messages advanced query: a malformed member query is rejected in the UI and holds Save/Send"
plane: experiential
question: What does the Push Messages admin blade show when an Advanced query is malformed, and are Save and Send held?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/push-message/preview-recipients
  - coordinate: /workspace/embedded-app/push-messages
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T07:23:23.142Z
    by: session:p12772
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T07:23:35.090Z
    by: session:p4480
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T11:52:21.835Z
    by: session:8402b2ed
    who: kutasinaelena
    note: "Re-observed with the phrase emails:\"a in the Advanced query textarea: POST /api/push-message/preview-recipients answered 400, the headline showed an em-dash with no number plus the text \"Recipients can't be counted for this audience — the search index rejected the query. Check it in Show generated query.\", and both Save and Send were disabled. Replacing the phrase with membertype:Employee returned \"Query is valid — 1 recipients.\" and re-enabled Save and Send. No POST or PUT /api/push-message fired at any point."
---
In the Push Messages admin blade (More > Push Messages > New > Audience > Advanced query), a malformed member query (emails:"a) makes POST /api/push-message/preview-recipients return HTTP 400. The blade then renders the recipient headline as an em dash instead of a number, the inline error "Recipients cannot be counted for this audience - the search index rejected the query. Check it in Show generated query." (the literal string uses a contraction and an em dash), and the caption "Custom query". Both Save and Send carry aria-disabled=true and the class vc-blade-toolbar-base-button--disabled, so a click cannot be delivered and no create POST/PUT /api/push-message fires. Replacing the text with a valid query (membertype:Contact) restores "Query is valid - N recipients." and re-enables both buttons. Observed on VirtoCommerce.PushMessages 3.1006.0-pr-28-21fe with vc-shell 2.6.1.
