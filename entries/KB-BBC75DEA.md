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
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T12:11:10.321Z
    by: session:8402b2ed
    who: kutasinaelena
    contradicts: true
    note: "On PushMessages 3.1006.0-pr-28-21fe picker options are no longer bare names: every option renders a 'Person' or 'Company' status badge before the name, and every Person option renders the member's email under it. Searching PUSH-PARENT returned 2 Company options (AGENT-TEST-PUSH-PARENT, AGENT-TEST-PUSH-CHILD - badge, no email because those orgs carry none) and 5 Person options each with badge + email, e.g. 'Person | Child Two | agent-test-push-child-2@yopmail.com'. A Company option DOES show an email when the organization has one (AGENT-TEST-Org-AcmeWest-20260310 shows acme-west@test-agent.com). The per-chip recipient count still appears only AFTER selection ('Company | AGENT-TEST-PUSH-PARENT | 3 recipients'), and genuinely duplicate organizations still render as identical indistinguishable rows - so the duplicate-entity half of this entry still holds; the 'no email, no type badge, no icon' half does not."
---
Blade: Push Messages > Audience > 'Add specific recipients (people or whole companies)' combobox. Each option is the bare display name only — no email, no type badge, no icon, no recipient count, no parent company, and no tooltip on hover. Same-named entities are therefore indistinguishable while choosing: searching 'John Mitchell' returns 8 fuzzy options including exactly two identical 'John Mitchell' rows (and two identical 'Emily Johnson'); 'AcmeCorp' returns 2 identical 'AGENT-TEST-Org-AcmeCorp-20260310'; 'AcmeWest' returns 4 identical 'AGENT-TEST-Org-AcmeWest-20260310'. The 'Person'/'Company' badge and the per-chip recipient count ('Person John Mitchell · 1 recipient') appear only AFTER the option is selected.
