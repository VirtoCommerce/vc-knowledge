---
id: KB-DBD2EE69
subject: Admin header global search shows "Nothing found" and stays there when, after a zero-result query, the whole query is replaced in a single input event, although the providers' requests return matches; typing key by key shows them
plane: experiential
question: Can the Admin SPA header global search show "Nothing found" for a query that has matches?
questions:
  - text: Why does Ctrl+K search say Nothing found for admin when users exist?
  - text: Global search stale empty state after pasting a new query
  - text: Does the Admin global search refresh results when the query is replaced at once?
status: active
appliesTo:
  - axis: role
    value: administrator
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/platform/security/users/search
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: db5e6e6e527dec12
    conditions: platform=3.1087.0-pr-3134-a3d2; browser=edge
    defect: suspected
    at: 2026-10-21T15:01:36.760Z
    by: session:638251b6
    who: kutasinaelena
---
vc-platform PR #3134 build, admin, 1920 px, Edge. Typed or filled "zzqqxxnothing" -> "Nothing found for ...". Then replaced the whole value in one input event (Playwright fill, equivalent to paste over a selection) with "admin" or "user:admin": POST /api/platform/security/users/search {"keyword":"admin"} returned 200 with 2 users, yet after 5-6 s the panel still read 'Nothing found for "admin"' with no Menu/Settings/Users/Roles groups, 3 of 3 attempts. After the same zero-result query, Ctrl+A and typing "admin" key by key showed Settings, Users (admin, b2badmin) and Roles. A replacement from a non-empty result state (user:admin -> role:store in one event) did show results. Root cause not isolated (globalSearch.js runSearch/applyGroups).
