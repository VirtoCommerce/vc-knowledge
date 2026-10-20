---
id: KB-6CA2F98B
subject: "On /compare the All/Differences switch filters rows client-side and the 'Differ: N of M rows' line counts the rows that differ out of all rows; below md the tabs and Clear category sit in one bar with no differ line"
plane: experiential
question: "How do the All / Differences tabs and the 'Differ: N of M rows' line on /compare behave, and where do they render on mobile?"
questions:
  - text: Does the Differences tab on /compare reload the page or call the API?
  - text: "What does 'Differ: 10 of 11 rows' count on the compare page?"
  - text: Is the differ line shown on mobile compare?
  - text: Where are Clear category and the row filter on the compare table at 390 px?
status: active
appliesTo:
  - axis: role
    value: guest
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; browser=chromium
    at: 2026-10-20T14:15:25.401Z
    by: session:d992339e
    who: Lenajava1
---
Guest, theme 3.1.0-pr-11-d67b-d67b0e0a, 5 Inkjet Printers in one tab (the tab caps at 5: a 6th Add to Compare shows 'Only 5 products from the same category can be compared'). At 1920 and 1280 the corner cell of the single compare table holds the All / Differences group, 'Differ: 10 of 11 rows' and 'Clear category'. All showed 11 rows. Differences showed 10, dropping only Export, which was 'No' for all 5. There was no navigation and no GraphQL request on the switch. Rows with '–' for some products count as differing. Once the header row is stuck under the app header, the compact row keeps only the All/Differences group (no differ line, no Clear category). The header row is a focusable scroller: arrow keys scroll it, the body follows, and the corner cell stays pinned. Below md (700 and 390) one bar above the table holds All/Differences and Clear category, there is no differ line, and the corner cell is empty. Resizing 1280 -> 700 -> 1280 kept exactly one of each control, and the differ line came back at 1280.
