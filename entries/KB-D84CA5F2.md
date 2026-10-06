---
id: KB-D84CA5F2
subject: "Push Messages builder: 'is any of' cannot build a multi-value phrase on an enum field"
plane: experiential
question: In the Push Messages audience builder, does the 'is any of' operator on a dropdown-backed field (Customer type, Status) produce a comma-separated multi-value phrase?
questions:
  - text: Can I target push notifications at several customer types at once using 'is any of'?
  - text: Push Messages audience builder is any of Customer type Status single-select membertype
  - text: Why is the 'is any of' phrase identical to the 'is' phrase for an enum field in the push audience builder?
  - text: Which push audience fields do accumulate multiple values under 'is any of'?
  - text: Why does the push audience value control show a raw JSON array like [ "Contact" ]?
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
  - coordinate: GET /api/push-message
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T15:00:22.230Z
    by: session:8402b2ed
    who: kutasinaelena
---
No. On the dropdown-backed fields Customer type and Status the value control stays SINGLE-select under 'is any of': picking a second option REPLACES the first, so the generated phrase can never hold more than one value and is byte-identical to the 'is' phrase (e.g. membertype:Organization after picking Contact then Organization; status:Approved). The control also renders the value as a raw JSON array literal, [ "Contact" ], instead of a chip or label. The reference-backed fields Role and Company behave differently and DO accumulate (roleid:<id1>,<id2>; parentorganizations:<guid1>,<guid2>), as do the free-text fields, where a comma typed into the value yields field:a,b. So 'is any of' is only reachable as a multi-value operator on ref and text fields, never on enum fields.
