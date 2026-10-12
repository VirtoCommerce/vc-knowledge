---
id: KB-88DFA29B
subject: "The returns list clamps an out-of-range page query parameter: -1, 0 and non-numeric show page 1, a page past the last is rewritten to the last page, on both the My returns and the organization tab"
plane: experiential
question: What does /account/returns show for page=-1, page=0, page=abc or a page number past the last page, on My returns and on the organization tab?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/returns
  - coordinate: /account/returns?scope=organization
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-12T09:01:00.940Z
    by: session:573e5582
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    at: 2026-10-12T09:53:36.895Z
    by: session:573e5582
    who: Lenajava1
    note: "REG-2026-10-12-1119: My returns with 10 pages - page=-1/0/abc show page 1 rows, page=9999 rewritten to page=10 (9 rows); organization tab with 5 pages - page=9999 rewritten to page=5 (7 rows); no empty state."
---
Theme 2.60.0-pr-2523-4263, 2026-10-12. Organization tab with totalCount 20 (10 per page): page=-1, page=0 and page=abc each render the first page's rows with page 1 current (the URL keeps the raw value); page=3 is rewritten to page=2 and shows the last page's rows. My returns with 8 returns (one page): page=-1, 0 and abc render the 8 rows; page=2 is rewritten to /account/returns and shows the 8 rows. 'There are no returns yet' never appears for a user who has returns.
