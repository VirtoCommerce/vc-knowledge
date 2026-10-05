---
id: KB-40B9AEF4
subject: member keyword search misses hyphenated organization names
plane: experiential
question: Does POST /api/members/search with keyword find an organization named like AGENT-TEST-Org-Kingsbridge-Imports-20260514?
status: active
appliesTo:
  - axis: surface
    value: platform-api
anchors:
  - coordinate: POST /api/members/search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T09:55:28.969Z
    by: session:51fcfb97
    who: kutasinaelena
---
No. On vcst_qa (2026-10-05) POST /api/members/search {memberType:'Organization', keyword:<full hyphenated name>} returned totalCount 0 for orgs that exist (GET /api/members/{id} reads them), and so did a single distinctive token of the name (Kingsbridge); the keyword 'AGENT' did return hits. A keyword-less paged search {memberType:'Organization', take:500, skip} listed all 217 orgs including every one the keyword missed. So a find-or-create keyed on a keyword lookup of such a name misses the existing org and creates a duplicate on every run; resolve by a pinned id (GET /api/members/{id}) or filter a keyword-less listing by exact name.
