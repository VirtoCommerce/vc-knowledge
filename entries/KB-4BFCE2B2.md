---
id: KB-4BFCE2B2
subject: "Push Messages audience builder: field to member-index phrase mapping"
plane: experiential
question: In the Push Messages back-office audience builder (Match by conditions, Custom conditions), what phrase does each field+operator emit into PushMessage.MemberQuery?
questions:
  - text: What query text does each condition in the push notification audience builder generate?
  - text: PushMessage.MemberQuery field tokens roleid parentorganizations createddate emails login
  - text: Does the Role or Company condition emit the name or the id into the member query?
  - text: How do starts with, contains and on or after operators look in the generated query?
  - text: When is a value double-quoted in the Push Messages generated query?
concepts:
  - id: push-audience
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pushmessages
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/push-message
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T15:00:08.804Z
    by: session:8402b2ed
    who: kutasinaelena
---
The Custom-conditions builder emits these member-index field tokens: Name -> name, Email -> emails, Username -> login, Customer type -> membertype, Status -> status, Tag -> groups, Business category -> businesscategory, Preferred language -> defaultlanguage, Role -> roleid (the role id, not the name), Company -> parentorganizations (the organization GUID, not the name), Belongs to a company -> hasparentorganizations, Registered -> createddate. Operator shapes: is -> field:value; is not -> !field:value; is any of -> field:a,b; starts with -> field:"a*"; ends with -> field:"*a"; contains -> field:"*a*"; on or after -> field:[YYYY-MM-DD TO]; on or before -> field:[TO YYYY-MM-DD]. A value containing whitespace is double-quoted, a value without whitespace is emitted bare. Read verbatim from the Show generated query dialog.
