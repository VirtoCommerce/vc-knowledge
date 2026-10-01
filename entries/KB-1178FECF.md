---
id: KB-1178FECF
subject: Push Messages audience resolution survives an ElasticAppSearch to ElasticSearch8 provider switch; only text-matched counts move
plane: experiential
question: Does switching the search provider to ElasticSearch8 and rebuilding the member index change the Push Messages audience-builder recipient counts?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pushmessages
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: /api/push-message/preview-recipients
  - coordinate: /api/push-message
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T14:39:26.020Z
    by: session:8402b2ed
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T15:00:57.944Z
    by: session:8402b2ed
    who: kutasinaelena
    note: "Walked the full 42-combination field x operator matrix through the Custom conditions builder on vcptcore-qa1 after the ElasticSearch8 switch (Member = 263 docs). All three wildcard operators return non-zero: emails:\"kutasina*\" -> 15 members, emails:\"*@gmail.com\" -> 20 members / 18 recipients, emails:\"*kutasina*\" -> 15 members, login:\"*@yopmail.com\" -> 20, name:\"Elena*\" / name:\"*Kutasina\" / name:\"*Kutas*\" -> 3 each. Exact emails:kutasina.elena@gmail.com correctly returns 0."
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T22:01:41.381Z
    by: session:8402b2ed
    who: kutasinaelena
    note: Re-verified on PushMessages 3.1006.0-pr-28-8de4 (commit 8de424a9) via suite 068 PUSH-049/PUSH-051. Parent org alone previews 3; unioned with 'Customer type is Employee' the estimate is 4 (caption 'Customers where Customer type is Employee, plus AGENT-TEST-PUSH-PARENT (whole company).'), and both survive Save + reopen from Drafts. Everyone still estimates 146 recipients (members matched 192 / people in scope 145 / extra logins +1); the preview dialog paged it as 7 full pages of 20 plus a final page of 6, labels '1-20 of 146' through '141-146 of 146', 146 rows collected with 146 distinct, no repeats and no blank last page.
---
Re-ran the Push Messages audience-builder UI cases on vcptcore-qa1 (VirtoCommerce.PushMessages 3.1006.0-pr-28-21fe) after the stand moved from ElasticAppSearch to ElasticSearch8 with all seven indexes rebuilt (Member index 263 documents, was 246). Every id-based audience number was IDENTICAL to the pre-switch run: picking a parent organization that holds one direct contact plus a child organization with two login contacts and one no-login contact previews 3; the child organization alone previews 2; the no-login contact alone previews 0; a company list unioned with a 'Customer type is Employee' condition previews 4. The estimate breakdown (members matched / N companies expanded / people in scope) is unchanged too. What DID move is text-derived: the 'Everyone' phrase membertype:Contact now estimates 146 recipients on this stand. The audience preview dialog pages the whole audience at 20 rows a page and reconciles to the headline - 146 came back as seven full pages plus a final page of 6, labels running '1-20 of 146' through '141-146 of 146', no truncation and no repeated rows. A malformed phrase still makes POST /api/push-message/preview-recipients answer HTTP 400, the estimate render an em-dash with the message 'Recipients can't be counted for this audience - the search index rejected the query', and both Save and Send go disabled; replacing it with a valid phrase restores the count and both buttons. For test design: audience RESOLUTION (recursive company expansion, the login gate, list-plus-condition union) is provider-independent, so its fixture counts can be asserted across a provider change, whereas any count derived from matching TEXT against the member index must be re-measured after a reindex.
