---
id: KB-0BE09FF7
subject: The dashboard My activity widget shows hour-bucket rows as '~N hours ago' for anything under 24 hours old, even when it fell on the previous calendar day; only from 24 hours on does it count calendar days ('~yesterday', '2 days ago')
plane: experiential
question: When does a row in the sales-rep dashboard My activity widget read '~yesterday' rather than '~N hours ago'?
questions:
  - text: Does an event from late yesterday show as yesterday or as hours ago in the My activity widget?
  - text: How is relative time computed for compact sales-rep activity rows?
  - text: Which timezone does the My activity widget use to decide 'yesterday'?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-my-activity-widget
  - axis: module
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: Query.salesRepActivities
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; browser-tz=Europe/Moscow; culture=en-US
    at: 2026-10-20T18:21:02.967Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc
    at: 2026-10-21T07:40:56.772Z
    by: session:68b36f2b
    bodyHash: a8f5f04b8dcf
    who: Aleksandra-Mitricheva
    note: "Intended behaviour: the feature owner confirmed on 2026-10-21 that '~N hours ago' under 24 h is acceptable and '~yesterday' applies only when the event is on the previous calendar day and at least 24 h old. Side effect of the rule: an event late in the evening shows '~yesterday' only briefly — e.g. Monday 23:30 reads '~23 hours ago' until Tuesday 23:30 and '2 days ago' from Wednesday 00:00."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    at: 2026-10-29T00:05:57.569Z
    by: session:ce3cfc2a
    bodyHash: a8f5f04b8dcf
    who: Aleksandra-Mitricheva
    note: "Partial: only platform (order) rows were present on vcst (GA reads down); an order placed ~9.4 h earlier read '9 hours ago' with no '~' - consistent with the entry's statement that only hour buckets get the '~' prefix."
---
Browser timezone Europe/Moscow (UTC+3), rep Priya. At 2026-10-20 18:17Z (21:17 local) the widget showed: bucket 2026-10-20 05Z '~13 hours ago', 2026-10-20 00Z '~18 hours ago', 2026-10-19 18Z (21:00 local, 24h17m back) '~yesterday', 2026-10-19 17Z '~yesterday'. Earlier the same day at 10:48Z the 2026-10-19 18Z bucket (previous calendar day in both UTC and local, 16h48m back) read '~16 hours ago'. Source (vc-module-sales-rep StorefrontApp utils.ts formatTimeAgo) picks the largest unit whose threshold the elapsed seconds reach (day = 86400 s); only when that unit is 'day' does it switch to calendar days counted on the browser's local clock (Intl.RelativeTimeFormat numeric auto -> 'yesterday'). Under 24 h it always prints hours, so a previous-calendar-day event less than 24 h old reads '~N hours ago'. Hour buckets get a '~' prefix and never read closer than 'this hour'.
