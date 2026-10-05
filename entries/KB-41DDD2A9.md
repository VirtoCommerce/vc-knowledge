---
id: KB-41DDD2A9
subject: "Push Messages builder: typing into the Registered date field surfaces a raw RangeError stack trace"
plane: experiential
question: What happens if a user types a date into the Registered condition's date input in the Push Messages audience builder instead of picking it from the calendar?
questions:
  - text: What happens if I type a date into the push notification audience 'Registered' filter?
  - text: Push Messages Registered condition 'Invalid time value' RangeError stack trace banner
  - text: Does the Invalid time value error banner go away after picking the date from the calendar?
  - text: What query does picking a Registered date from the calendar produce?
concepts:
  - id: push-audience
  - id: admin-validation
  - id: date-range-picker
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
    at: 2026-09-30T15:00:47.221Z
    by: session:8402b2ed
    who: kutasinaelena
---
Typing a date (e.g. 2026-01-01) into the Registered condition's 'Select date' input leaves the field empty and raises a blade-level red error banner reading 'Invalid time value'; expanding it shows an untranslated JavaScript stack trace — 'RangeError: Invalid time value at z (.../vc-shell-framework...) at Cn (.../vc-shell-vendor-vuepic...)' — i.e. the @vuepic datepicker's parse error is surfaced verbatim to the operator. The banner then PERSISTS for the life of the blade: it is still shown after the date is picked successfully from the calendar and after the condition's field is changed away from Registered altogether. Picking the date from the calendar works and produces createddate:[2026-01-01 TO].
