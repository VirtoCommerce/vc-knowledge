---
id: KB-E3BEB939
subject: /company/tasks?filter=<unknown token> for a sales rep with tasks falls back to the Today scope and cleans the URL to /company/tasks; a calendar day pick from a tab also drops ?filter
plane: experiential
question: What does /company/tasks do with an unknown ?filter value for a sales rep who has tasks?
questions:
  - text: Does /company/tasks?filter=zzz show an empty list or a fallback scope?
  - text: Is the filter query removed from the URL when a day is picked on /company/tasks?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
  - coordinate: Query.salesRepTasks
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; theme=2.61.0-pr-2576-be2c; browser=firefox
    at: 2026-10-27T20:20:58.746Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
    note: "distinct: KB-A915AA45 is a rep with zero tasks / no served orgs where the status chips are absent; this is a rep WITH tasks and all chips, covering unknown-token fallback, valid deep links and the day-pick URL rewrite."
---
Signed in as a sales rep with 22 tasks (served organizations present, all five chips rendered). Loading /company/tasks?filter=zzz: during load the Today chip briefly reads 'Today 0', then the page settles on the Today chip pressed with its real count (7) and 7 rows, and the URL is rewritten to /company/tasks (filter=zzz gone). /company/tasks?filter=overdue selects Overdue and keeps the query. From ?filter=upcoming, clicking a non-today calendar day opens the date chip and rewrites the URL to /company/tasks; browser Back then lands on /company/tasks with Today pressed (no ?filter history entry left).
