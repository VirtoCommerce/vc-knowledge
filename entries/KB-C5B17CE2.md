---
id: KB-C5B17CE2
subject: Push Messages audience caption omits the condition when the blade is rendered from saved state
plane: experiential
question: Does the Push Messages audience caption name the condition when a message combines a condition with a picked recipient?
questions:
  - text: After I saved a push message, why does the audience summary only mention the one person I picked?
  - text: Is the push message audience caption reliable after reopening a message that mixes a condition and explicit recipients?
  - text: Does the stored member query and generated query stay correct while the estimate caption drops the condition?
  - text: What makes the push audience caption re-derive correctly after saving?
  - text: Does the push send job union the condition audience with explicitly selected members?
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
  - coordinate: GET /api/push-message/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-28T14:59:19.522Z
    by: session:p28140
    who: kutasinaelena
---
Blade: Push Messages > Audience > estimate panel. With BOTH a condition and an explicitly picked person, the one-line audience caption is correct only while the audience is being edited. Built live it reads 'Customers where Customer type is Employee, plus John Mitchell.'; after Save, and again after closing and reopening the message from the list, the SAME audience renders as 'Sending to John Mitchell.' — the condition is omitted although the condition row is visibly present and the headline reads 2 recipients / members matched 3 / people in scope 2. Touching any condition control re-derives the caption and it becomes correct again. Stored data and 'Show generated query' are correct throughout (membertype:Employee + 'Plus 1 explicitly selected in MemberIds. The send job unions the two sets.'). The caption understates the audience — a display-only defect whose error direction is permissive.
