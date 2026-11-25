---
id: KB-A3E4CB56
subject: PUT api/admin-shell/dashboard (global, personalization, roles/{id}, me) with a non-JSON or non-binding body answers 400 and writes nothing; a missing role answers 404
plane: experiential
question: What do PUT /api/admin-shell/dashboard/global, /personalization, /roles/{roleId} and /me answer to a malformed body or an unknown role id?
status: active
appliesTo:
  - axis: role
    value: administrator
  - axis: surface
    value: rest-api
anchors:
  - coordinate: PUT /api/admin-shell/dashboard/global
  - coordinate: PUT /api/admin-shell/dashboard/roles/{id}
  - coordinate: GET /api/admin-shell/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; module:VirtoCommerce.AdminShell=3.1101.0
    at: 2026-11-25T23:16:03.838Z
    by: session:779b94fc
    who: kutasinaelena
---
As administrator on a stand with nothing configured, PUT with body "not json" to /global, /personalization, /me and /roles/no-such-role-id returned 400 with an errors object naming the parse error; PUT /global with {"widgets":"x"} returned 400 naming the widgets conversion; PUT /roles/no-such-role-id with a valid {"widgets":{}} body returned 404 with an empty body. GET /global, /personalization and /me were identical before and after. GET /api/admin-shell/dashboard without a token returned 401. Defaults seen: GET /api/admin-shell/dashboard returned {defaults, hidden:[], layout:{}, personalization:{allowMove:true,allowResize:true}, layoutVersion:0, canManage:true}; GET /roles returned a list of {id,name,permissions,dashboard:{widgets,layoutVersion}}. Contrast with the menu endpoints (KB-5A2B8DA9), which accept a malformed body.
