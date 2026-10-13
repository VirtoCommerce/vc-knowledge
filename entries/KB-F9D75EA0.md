---
id: KB-F9D75EA0
subject: VcDateRangePicker footer Clear scope differs between split and combined layouts
plane: experiential
question: In the storefront date range picker, does the calendar footer Clear empty both dates or only one?
questions:
  - text: How do I clear both dates in the order history date filter?
  - text: Does the calendar footer Clear reset the whole range or only one field in the date range picker?
  - text: Why does the date range filter behave differently when clearing on a narrow screen?
  - text: What accessible names do the calendar triggers have in the split and combined date range layouts?
concepts:
  - id: date-range-picker
  - id: mobile-layout
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-29T15:28:41.618Z
    by: session:79182e15
    who: Lenajava1
    build: a7d3cf1036046bf9
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T20:05:38.826Z
    by: session:414d7399
    who: kutasinaelena
    contradicts: true
    note: "Trigger-name clause only: on 2026-10-02 (Theme 2.59.0-pr-2524, regression case ORD-076, desktop/split layout of /account/orders Filters) both calendar triggers exposed the identical accessible name 'Open calendar', not 'Open calendar: Start date' / 'Open calendar: End date'. vc-frontend dev (2026-10-02) source agrees: vc-date-picker.vue sets :aria-label=\"t('ui_kit.accessibility.open_calendar')\" with no field suffix, used for both Start and End in the split branch of vc-date-range-picker.vue. The Clear-scope part of the entry was not re-checked."
    resolved: version-scoped
    resolvedAt: 2026-10-12T18:27:45.626Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: "Both sides are right on their own build: pr-2524 predates vc-frontend 702d1c1c (#2519), which added the field suffix to the split calendar triggers; builds with it show 'Open calendar: Start date' / 'End date', re-observed on vcst and vcptcore."
    build: 59983c56933fe571
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T15:01:51.380Z
    by: session:7982e016
    who: Aleksandra-Mitricheva
    note: "Split-layout part only (Clear scope not re-checked). On theme 2.59.0-pr-2519 at 1920px, /account/orders Filters: each field opens its own SINGLE-month calendar (one 42-cell grid, Previous/Next year+month, Today/Clear footer), and the triggers ARE named 'Open calendar: Start date' / 'Open calendar: End date' on this build, so whether the trigger name has a suffix depends on the build (the dispute was seen on pr-2524)."
    build: 2077d219738578a0
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2511-81f1; role=sales-rep
    at: 2026-10-12T18:27:41.563Z
    by: session:34990ccd
    who: Dan-BV
    note: "2026-10-12 Filters on /account/orders and /company/my-customers/<id>/orders: split (1920) triggers 'Open calendar: Start date' / 'Open calendar: End date'; combined (375) one 'Open calendar'; split footer Clear empties only its field (both directions), combined Clear empties both; no in-field clear. Source: vc-date-range-picker.vue L28/L49 open_calendar_for, added by 702d1c1c (#2519)."
    investigation: true
    bodyHash: 340571338a00
    build: 4ca08b7825765bae
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    conditions: theme=2.60.0-pr-2522-6f90; role=sales-rep
    at: 2026-10-12T18:27:43.535Z
    by: session:34990ccd
    who: Dan-BV
    note: "2026-10-12 Filters on /account/orders and /company/my-customers/<id>/orders: split (1920) triggers 'Open calendar: Start date' / 'Open calendar: End date'; combined (375) one 'Open calendar'; split footer Clear empties only its field (both directions), combined Clear empties both; no in-field clear. Source: vc-date-range-picker.vue L28/L49 open_calendar_for, added by 702d1c1c (#2519)."
    investigation: true
    bodyHash: 340571338a00
    build: c55c1a124b302d3b
    buildFrom: deploy-history
---
In the split layout (two labelled fields, used at sm/640px and wider) each field opens its own calendar, and that calendar's footer Clear empties only its own field; emptying the range needs Clear in both calendars. In the combined layout (one Date range field, below sm) the single calendar's footer Clear empties both dates at once. The split fields carry no in-field clear button; their calendar triggers are named 'Open calendar: Start date' / 'Open calendar: End date', while the combined trigger is named just 'Open calendar'. The suffixed split trigger names come from vc-frontend 702d1c1c (VCST-6001, #2519, merged 2026-10-07: calendarButtonAriaLabel + ui_kit.accessibility.open_calendar_for); builds without it, such as 2.59.0-pr-2524 (2026-10-02), name both split triggers just 'Open calendar'. Re-observed 2026-10-12 on themes 2.60.0-pr-2511-81f1 (vcst) and 2.60.0-pr-2522-6f90 (vcptcore), on /account/orders and the sales-rep customer orders Filters (/company/my-customers/<id>/orders): suffixed names in split at 1920 px, one 'Open calendar' in combined at 375 px, split Clear empties only its own field, combined Clear empties both, no in-field clear.
