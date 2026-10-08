---
id: KB-EB70C1F9
subject: "Push Messages conditions builder: an incomplete condition row was dropped silently on the 2026-09-28 build; from PushMessages 3.1006.0-pr-28 it shows \"This condition has no value.\" and holds Save and Send"
plane: experiential
question: In the Push Messages admin blade (Match by conditions), what happens to a condition row whose value is left empty?
questions:
  - text: If I leave a condition value blank when targeting a push message, who ends up receiving it?
  - text: Does the push message audience builder warn about a condition row with no value before saving?
  - text: What member query is stored when a match-all condition row has an empty value?
  - text: Can an admin still save or send a push campaign while one audience condition is incomplete?
  - text: Why is the recipient estimate identical with and without an empty custom condition?
concepts:
  - id: push-audience
  - id: admin-validation
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pushmessages
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/push-message
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-28T14:59:08.297Z
    by: session:p15752
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T07:22:47.779Z
    by: session:p16548
    who: kutasinaelena
    contradicts: true
    note: "On PushMessages 3.1006.0-pr-28-21fe the incomplete condition row is no longer silently dropped: the builder renders the inline hint 'This condition has no value.' under the offending row and both Save and Send become genuinely inert (aria-disabled=true, class vc-blade-toolbar-base-button--disabled; a Playwright click cannot be delivered and no POST/PUT /api/push-message fires). Removing the empty row re-enables Save immediately."
    resolved: version-scoped
    resolvedAt: 2026-10-08T11:15:38.511Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/push-messages-20261008
    resolution: Hint and held Save/Send from 3.1006.0-pr-28, live on the released 3.1008.0 on two stands; the silent drop belonged to the 2026-09-28 build. Body scoped by build.
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1077.0-pr-3123-6664; module:VirtoCommerce.PushMessages=3.1008.0; module:VirtoCommerce.Customer=3.1028.0; vc-shell=2.6.1; role=Administrator
    at: 2026-10-08T11:15:33.500Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: complete condition plus an empty-value row -> hint \"This condition has no value.\", Save and Send disabled, no POST; value typed -> enabled; cleared -> held again; row removed -> Save enabled. Starter row alone: no hint, Save enabled, Send disabled."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.PushMessages=3.1008.0; module:VirtoCommerce.Customer=3.1029.0; vc-shell=2.6.1; role=Administrator
    at: 2026-10-08T11:15:34.309Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: same as vcptcore_qa1."
---
What the audience conditions builder ("Match by conditions" > Custom conditions) does with a row that has no value depends on the build.

On the build observed 2026-09-28 an incomplete row was dropped silently: no validation message, Save not held, and POST /api/push-message saved only the complete conditions.

From PushMessages 3.1006.0-pr-28 (and on the released 3.1008.0): adding a row with an empty value under a complete condition renders the joiner and the inline hint "This condition has no value." under that row, and Save and Send are both disabled - no POST is sent. Typing a value removes the hint and enables them; clearing it brings the hint back; removing the empty row re-enables Save at once. The estimate and the caption ("Customers where Customer type is Contact.") ignore the empty row throughout. The starter row on its own (Custom conditions with one empty row) is a different state: no hint, Save enabled, Send disabled, "0 recipients" / "No conditions set". Observed live 2026-10-08 on PushMessages 3.1008.0 (vc-shell 2.6.1) on vcptcore_qa1 and vcst_qa.
