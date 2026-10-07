---
id: KB-EA4DA051
subject: On /company/tasks (theme 2.59.0-pr-2536) the es and ru row action label 'Mark as complete' wraps to two centred lines in a 208px button at both 1440 and 1920px
plane: experiential
question: Does the Mark as complete row action on /company/tasks fit on one line in es and ru?
status: active
appliesTo:
  - axis: locale
    value: es-es,ru-ru
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /es/company/tasks
  - coordinate: /ru/company/tasks
  - coordinate: /es/company/calendar
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T13:29:09.320Z
    by: session:5e2a1103
    who: Lenajava1
---
On /es/company/tasks and /ru/company/tasks at 1440 and 1920px the Actions column is a fixed 224px (w-56) and the row button is 208px wide, 38px tall. The labels 'Marcar como completada' and 'Отметить как выполненную' wrap to two lines (label box 32px at 16px line-height), centre-aligned, while the leading check icon stays at the left (16px in, vertically centred), so the icon sits apart from the visually centred text. 'Reabrir' / 'Возобновить' fit on one line. The old /company/calendar page on theme 2.59.0 had no row action button: completion was a 40px checkbox column (es header 'Completada').
