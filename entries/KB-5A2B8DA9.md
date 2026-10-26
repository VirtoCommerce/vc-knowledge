---
id: KB-5A2B8DA9
subject: PUT /api/admin-shell/menu/global (and roles/{roleId}, me/prefs) with a malformed or non-JSON body answers 204 and stores an empty patch, wiping the existing layer
plane: experiential
question: What happens when PUT /api/admin-shell/menu/global receives an invalid JSON body?
questions:
  - text: Does the Admin Shell menu API validate the patch body?
  - text: Can a bad request reset the global Admin Shell menu customization?
  - text: What does PUT /api/admin-shell/menu/roles/{roleId} do with an unparseable body?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.adminshell
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/admin-shell/menu/global
  - coordinate: PUT /api/admin-shell/menu/roles/{id}
  - coordinate: PUT /api/admin-shell/menu/me/prefs
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481; role=administrator
    defect: suspected
    at: 2026-10-23T09:19:34.576Z
    by: session:5de4e186
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481; role=administrator
    at: 2026-10-23T11:40:53.593Z
    by: session:5de4e186
    bodyHash: 3c437e068894
    who: kutasinaelena
    note: "Re-observed in regression run: with a stored rename patch, PUT menu/global raw 'not-json' and {\"sections\": []} each -> 204 and GET returned the empty patch; roles/{id} and me/prefs non-JSON -> 204 and wiped a stored hide / pin."
  - method: observation
    deployment: vcptcore_qa1
    build: 4a14ffabb4298717
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-f481; role=administrator
    at: 2026-10-23T12:51:22.789Z
    by: session:5de4e186
    bodyHash: 3c437e068894
    defect: suspected
    who: kutasinaelena
    note: "Triage re-run: non-JSON text, {\"sections\": []} and {\"foo\":1} each -> 204 and an empty layer on global, roles/{id} (throwaway role) and me/prefs (9/9); text/plain -> 415 and unchanged. For comparison PUT /api/platform/security/roles with a non-JSON body -> 500 NullReference, so platform core does not answer 400 there either."
  - method: observation
    deployment: vcptcore_qa1
    build: 3c977d7c206bc258
    conditions: platform=3.1086.0-pr-3137-8aa4; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-34b3; role=administrator
    at: 2026-10-26T11:11:08.064Z
    by: session:5de4e186
    bodyHash: 3c437e068894
    who: kutasinaelena
    contradicts: true
    contested: module:VirtoCommerce.AdminShell 3.1000.0-pr-1-34b3 vs 3.1000.0-pr-1-f481; virtocommerce.adminshell 3.1000.0-pr-1-34b3 vs 3.1000.0-pr-1-f481
    note: "On AdminShell 3.1000.0-pr-1-34b3, with application/json: PUT /api/admin-shell/menu/global raw 'not-json' -> 400 problem+json ('Unexpected character...'), {\"sections\": []} -> 400 (cannot deserialize array into Dictionary<string,SectionPatch>), {\"sections\":{\"orders\":\"x\"}} -> 400; PUT roles/{id} {\"sections\": []} -> 400; PUT me/prefs {\"pinned\":\"x\"} -> 400. GET afterwards returned the unchanged stored layers and the effective menu was unchanged. Malformed bodies are now refused; the entry holds only for build pr-1-f481."
---
As administrator, with a non-empty global patch stored (8 item overrides, 2 added sections, 2 added items), PUT /api/admin-shell/menu/global was sent with Content-Type application/json and bodies: plain text, truncated JSON '{ "sections": ', wrong shape {"sections":[],"items":"x"}, literal null and an empty body. Every one answered 204 No Content, and GET /api/admin-shell/menu/global then returned the empty patch {"sections":{},"items":{},"addSections":[],"addItems":[]} — the whole stored customization was replaced. Only a non-JSON content type (text/plain) was refused, with 415. The same holds for PUT /api/admin-shell/menu/roles/{roleId} (a stored hide was wiped) and PUT /api/admin-shell/menu/me/prefs (a user's pins were wiped). Source: the controller actions are on a plain Controller (no ApiController attribute), so model binding yields null and the action saves `patch ?? new MenuPatch()`. For comparison, POST /api/platform/security/login with a malformed body answers 400. The Menu Editor's own JSON view refuses invalid JSON client-side (Apply disabled).
