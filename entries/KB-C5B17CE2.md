---
id: KB-C5B17CE2
subject: "Push Messages audience caption: on the 2026-09-28 build it collapsed to the picked recipients when the blade was rendered from saved state; from PushMessages 3.1006.0-pr-28 it keeps the condition"
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
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T11:51:09.394Z
    by: session:8402b2ed
    who: kutasinaelena
    contradicts: true
    note: On PushMessages 3.1006.0-pr-28-21fe / vc-shell 2.6.1 the caption no longer collapses. With a Custom condition 'Customer type is Employee' plus a picked company, the caption read 'Customers where Customer type is Employee, plus AGENT-TEST-PUSH-PARENT (whole company).' while editing, and after Save + reopening the draft from the Drafts list it still read exactly that, with the headline still 4 recipients and the company chip intact. No 'Sending to X.' collapse, and no need to touch a condition control to re-derive it. Looks fixed since the 2026-09-28 build this entry was taken on; superseded by KB-D9F48037.
    resolved: version-scoped
    resolvedAt: 2026-10-08T11:15:39.331Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/push-messages-20261008
    resolution: No collapse from 3.1006.0-pr-28, live on 3.1008.0 on two stands including a full reload; the collapse belonged to the 2026-09-28 build.
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1077.0-pr-3123-6664; module:VirtoCommerce.PushMessages=3.1008.0; module:VirtoCommerce.Customer=3.1028.0; vc-shell=2.6.1; role=Administrator
    at: 2026-10-08T11:15:35.122Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: Employee condition plus a picked person saved as Draft (memberQuery membertype:Employee + memberIds); caption \"Customers where Customer type is Employee, plus <name>.\" after Save, after reopening from Drafts and All messages, and after a full reload via deep link."
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; module:VirtoCommerce.PushMessages=3.1008.0; module:VirtoCommerce.Customer=3.1029.0; vc-shell=2.6.1; role=Administrator
    at: 2026-10-08T11:15:35.971Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: same after a full reload via deep link."
---
With a custom condition plus a picked recipient (e.g. "Customer type is Employee" plus a person), the audience caption reads "Customers where Customer type is Employee, plus <name>." while editing.

On the build observed 2026-09-28, once the blade was rendered from saved state (after Save, or reopening the draft) the caption collapsed to "Sending to <name>." and omitted the condition until a condition control was touched.

From PushMessages 3.1006.0-pr-28 (and on the released 3.1008.0) it does not collapse: the saved draft (memberQuery "membertype:Employee" plus memberIds) re-renders with the full caption, the same recipient headline and the chip intact - right after Save, after closing and reopening from Drafts or All messages, and after a full page reload via a deep link to the details page - without touching any condition control. Observed live 2026-10-08 on PushMessages 3.1008.0 (vc-shell 2.6.1) on vcptcore_qa1 and vcst_qa.
