---
id: KB-BBC75DEA
subject: Push Messages recipient picker renders same-named people and companies with no discriminator
plane: experiential
question: Does the Push Messages 'Add specific recipients' picker distinguish options that share a display name?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pushmessages
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/push-message/search-recipients
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-28T14:59:31.173Z
    by: session:p24252
    who: kutasinaelena
---
Blade: Push Messages > Audience > 'Add specific recipients (people or whole companies)' combobox. Each option is the bare display name only — no email, no type badge, no icon, no recipient count, no parent company, and no tooltip on hover. Same-named entities are therefore indistinguishable while choosing: searching 'John Mitchell' returns 8 fuzzy options including exactly two identical 'John Mitchell' rows (and two identical 'Emily Johnson'); 'AcmeCorp' returns 2 identical 'AGENT-TEST-Org-AcmeCorp-20260310'; 'AcmeWest' returns 4 identical 'AGENT-TEST-Org-AcmeWest-20260310'. The 'Person'/'Company' badge and the per-chip recipient count ('Person John Mitchell · 1 recipient') appear only AFTER the option is selected.
