---
id: KB-DF9F415E
subject: "Admin Returns: user without return:authorize gets no Approve / decline command"
plane: experiential
question: What does the Admin SPA Returns Line items blade show to a back-office user holding return:update but not return:authorize?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/return/{id}/authorize
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-29T07:02:44.193Z
    by: session:p25028
    who: kutasinaelena
---
With return:access/read/update and no return:authorize, the Return Line items blade shows no Approve / decline toolbar command, the Approved column is plain read-only text (0) and Decline reason has no input; Quantity stays editable and Save enables after a change. The return details Status dropdown for a Requested return offers only Requested. Administrator on the same return sees Approve / decline, an Approved input pre-filled with the requested quantity, and a Decline reason field. Role picker lists return:authorize under Return as 'Approve and decline returns'.
