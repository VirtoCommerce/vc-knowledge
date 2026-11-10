---
id: KB-6EE8A17D
subject: The Admin Shell defaults to Dark appearance and English for a user with no saved preference; Appearance and Language are saved per user as platform settings
plane: experiential
question: What appearance and language does the Admin Shell use for a user who never changed them, and where is the choice stored?
questions:
  - text: Is the Admin Shell dark by default?
  - text: Where does the Admin Shell store the Appearance (light/dark) choice?
  - text: Does changing the Admin Shell language persist per user on the server?
status: active
appliesTo:
  - axis: surface
    value: admin-shell
anchors:
  - coordinate: /apps/admin-shell
  - coordinate: POST /api/platform/settings/v2/me/values
  - coordinate: GET /api/platform/settings/v2/me/values
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: platform=3.1096.0
    at: 2026-11-10T20:38:41.993Z
    by: session:ba89ac2a
    who: Lenajava1
---
On a fresh isolated browser context the Admin Shell login page renders dark. Signed in as two users whose GET /api/platform/settings/v2/me/values?moduleId=VirtoCommerce.AdminShell&modifiedOnly=true returned {}, the Account menu showed Appearance = Dark (explicitly checked, not 'Match the system') and Language = English. The setting AdminShell.UI.Appearance has defaultValue 'dark' (allowed system/light/dark) and VirtoCommerce.Platform.UI.Language defaultValue 'en'. Choosing Light or another language in the Account menu POSTs /api/platform/settings/v2/me/values with {"AdminShell.UI.Appearance":"light","VirtoCommerce.Platform.UI.Language":"en"}, i.e. both are stored server-side per user and survive a new browser context. A test that 'switches to dark' starts from the default here.
