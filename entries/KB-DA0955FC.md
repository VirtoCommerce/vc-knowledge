---
id: KB-DA0955FC
subject: "Page Builder publish-conflict classification observed live: group superset over subset is Certain, subset over superset Possible, touching dates Possible, +1s None; check-then-act race can publish two Certain pages"
plane: experiential
question: How does POST /api/page-builder-pages/grouped/permalink-usages classify user-group direction, touching dates and concurrent publishes?
questions:
  - text: Is publishing {vip,gold} over a published {gold} page a Certain conflict?
  - text: Do touching dates count as overlapping for permalink conflicts?
  - text: Can two concurrent publishes of Certain-conflicting pages both succeed?
  - text: Does an any-language page conflict with a store-default-language page?
status: active
appliesTo:
  - axis: module
    value: page-builder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/page-builder-pages/grouped/permalink-usages
  - coordinate: POST /api/page-builder-pages/grouped/publishing/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 23e5874137c52ea7
    conditions: module:VirtoCommerce.PageBuilderModule=3.1039.0-pr-179-5c9d; platform=3.1091.0-pr-3144-e637; store-default-language=en-US
    at: 2026-10-28T14:51:21.285Z
    by: session:44c579c3
    who: Aleksandra-Mitricheva
---
Observed on one store (default en-US), same permalink, same org/language: new {vip,gold} vs published {gold} -> Certain, publish without flag 409; new {gold} vs published {vip,gold} -> Possible, publish 200 and both stay Published; {VIP} vs {gold} -> Possible. Other page ends at T, new page starts at T -> Possible; starts T+1s -> None (inclusive bounds). Any-language evergreen vs en-US dated -> Possible; any-language vs de-DE -> None. Permalink match is case-insensitive and leading-slash-insensitive ("Agent-Test-X" vs "/agent-test-x" -> Certain); whitespace and "//" are rejected at create with 400. Two Certain-equal drafts published concurrently without unpublishConflicting: 3 of 4 trials 200+409, 1 trial 200+200 leaving both Published (no lock between check and save). Draft rows in usages report canUnpublish=false.
