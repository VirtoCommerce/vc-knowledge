---
id: KB-8C054BAE
subject: In the pt (pt-PT) storefront locale, VcCalendar weekday headers render full words (domingo, segunda...) that overflow and overlap their cells, on every calendar instance
plane: experiential
question: Do the VcCalendar weekday header labels fit their cells in the pt locale on the sales-rep calendar/tasks, dashboard Tasks widget and customer-orders date picker?
status: active
appliesTo:
  - axis: component
    value: vccalendar
  - axis: locale
    value: pt-pt
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /pt/company/calendar
  - coordinate: /pt/company/tasks
  - coordinate: /pt/company/dashboard
  - coordinate: /pt/company/customer-orders
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-07T13:29:09.279Z
    by: session:5e2a1103
    who: Lenajava1
---
Signed in as a sales rep at 1920px, html lang pt-PT. The VcCalendar weekday header (reka CalendarRoot weekDays, weekdayFormat 'short' default) shows Intl short weekdays, which for pt-PT are full words: domingo, segunda, terça, quarta, quinta, sexta, sábado (pt-BR would give dom., seg., ...). Rendered uppercase bold with letter-spacing, they overflow fixed cells: md size (40px cells, 12px text) scrollWidth domingo 53, segunda 51, quarta 46, quinta 45, sábado 47, terça 41 vs clientWidth 40; sm size (32px cells, 10px text) domingo 43, segunda 42, quarta 38, sábado 38, quinta 37, terça 33 vs 32. Adjacent labels visibly run into each other. Measured identically on theme 2.59.0 (/pt/company/calendar page, /pt/company/dashboard Tasks widget, /pt/company/customer-orders Filters > Created date custom range > Open calendar) and on theme 2.59.0-pr-2536 (same widget and picker, plus the sm calendar in the /pt/company/tasks rail). So it is a pre-existing ui-kit calendar behaviour, not specific to one consumer. es-ES (dom, lun...) and ru-RU (вс, пн...) short weekdays are 2-3 letters and do not overflow.
