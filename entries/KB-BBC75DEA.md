---
id: KB-BBC75DEA
subject: "Push Messages recipient picker: same-named people and companies are indistinguishable; from PushMessages 3.1006.0-pr-28 options carry a Person / Company badge and the email, but nothing tells duplicates apart"
plane: experiential
question: Does the Push Messages 'Add specific recipients' picker distinguish options that share a display name?
questions:
  - text: When I pick push notification recipients, how do I tell apart two people with the same name?
  - text: In the back office push message audience, do dropdown options show email, type or company before selection?
  - text: Does the search-recipients picker render only the display name for each option, without a discriminator?
  - text: When does the Person or Company badge and recipient count appear on a picked push recipient?
  - text: Can duplicate-named companies be distinguished in the specific recipients combobox of a push message?
concepts:
  - id: push-audience
  - id: push-message
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pushmessages
  - axis: surface
    value: admin-ui
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
    resolved: version-scoped
    resolvedAt: 2026-10-08T11:15:40.161Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/push-messages-20261008
    resolution: Badge and email from 3.1006.0-pr-28, live on 3.1008.0; duplicates still indistinguishable, as the dispute itself said. Body scoped by build.
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1077.0-pr-3123-6664; module:VirtoCommerce.PushMessages=3.1008.0; module:VirtoCommerce.Customer=3.1028.0; vc-shell=2.6.1; role=Administrator
    at: 2026-10-08T11:15:36.841Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: options carry Person/Company badge and email; two same-named people with the same email and four same-named companies render as identical rows; no org/parent/account shown, no tooltip, count only after selection; picker calls POST /api/members/search."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.PushMessages=3.1008.0; module:VirtoCommerce.Customer=3.1029.0; vc-shell=2.6.1; role=Administrator
    at: 2026-10-08T11:15:37.698Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: badge and email on every option; no duplicate entities on this stand to compare."
---
The picker "Also add specific recipients (people or whole companies)" searches POST /api/members/search (keyword, deepSearch, objectType Member, sort MemberType:desc;Name).

On the build observed 2026-09-28 every option was the bare display name: no email, no type badge, no icon.

From PushMessages 3.1006.0-pr-28 (and on the released 3.1008.0) every option renders a "Person" or "Company" badge before the name and the member's email after it when the member has one. What still does not tell entities apart: two different members with the same name and the same email render as identical rows - on vcptcore_qa1 two "Person | John Mitchell | <same email>" options (one a contact in an organization with an account, one an org-less contact without one) and four identical "Company | AGENT-TEST-Org-AcmeWest-20260310 | <same email>" options with different parents. The picker shows no organization, parent company or account, there is no tooltip on hover, and the recipient count appears only on the chip after selection. Observed live 2026-10-08 on PushMessages 3.1008.0 (vc-shell 2.6.1) on vcptcore_qa1 and vcst_qa.
