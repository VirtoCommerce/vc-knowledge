---
id: KB-EB70C1F9
subject: Push Messages conditions builder drops an incomplete condition row without validation
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
---
Blade: Push Messages > New > Audience > Match by conditions > Custom conditions. An incomplete condition row (field + operator set, value empty) is silently dropped from the generated member query. With Match=ALL and rows 'Customer type is Contact' + 'Name is <empty>', the recipient estimate equals the Contact-only audience (129 recipients / 173 members matched / 128 people in scope) and the caption reads 'Customers where Customer type is Contact.' — the empty row is not mentioned. No validation message renders. Save is NOT held: POST /api/push-message returns 200 and persists memberQuery='membertype:Contact', i.e. the empty AND-row is dropped from the stored query. The error direction is permissive — the saved audience is wider than the builder appears to declare.
