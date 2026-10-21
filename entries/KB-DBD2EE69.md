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
  - method: observation
    deployment: vcptcore_qa1
    build: db5e6e6e527dec12
    conditions: platform=3.1087.0-pr-3134-a3d2; browser=edge; role=Administrator
    at: 2026-10-21T20:19:00.894Z
    by: session:638251b6
    bodyHash: 696e43c3c4a8
    who: kutasinaelena
    contradicts: true
    note: "A real paste does NOT reproduce it. Same build, admin, Edge 1920: after \"zzqqxxnothing\" showed \"Nothing found\", Ctrl+A then Ctrl+V of \"admin\" (real clipboard paste) showed the Settings result and Users/Roles groups loading - no stale \"Nothing found\". Only Playwright locator.fill(\"admin\") (programmatic value set) left 'Nothing found for \"admin\"'. The entry's premise \"fill is equivalent to paste over a selection\" is wrong; the operator also could not reproduce it by hand. Treat the original observation as a test-instrument artifact, not a product defect."
---
vc-platform PR #3134 build, admin, 1920 px, Edge. Typed or filled "zzqqxxnothing" -> "Nothing found for ...". Then replaced the whole value in one input event (Playwright fill, equivalent to paste over a selection) with "admin" or "user:admin": POST /api/platform/security/users/search {"keyword":"admin"} returned 200 with 2 users, yet after 5-6 s the panel still read 'Nothing found for "admin"' with no Menu/Settings/Users/Roles groups, 3 of 3 attempts. After the same zero-result query, Ctrl+A and typing "admin" key by key showed Settings, Users (admin, b2badmin) and Roles. A replacement from a non-empty result state (user:admin -> role:store in one event) did show results. Root cause not isolated (globalSearch.js runSearch/applyGroups).
