---
id: KB-8A8FBB97
subject: Admin SPA main menu collapsed state is stored per user in the MainMenu.State profile setting and survives reload
plane: experiential
question: Where is the Admin SPA main menu collapsed/expanded state persisted (POST /api/platform/profiles/currentuser, MainMenu.State)?
questions:
  - text: Does the admin menu stay collapsed after reload?
  - text: Which setting stores the admin main menu collapse state?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/platform/profiles/currentuser
  - coordinate: /api/platform/settings/v2/me/values
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: db5e6e6e527dec12
    conditions: platform=3.1087.0-pr-3134-a3d2; role=Administrator
    at: 2026-10-21T13:57:54.708Z
    by: session:638251b6
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 2dae523e983e5095
    conditions: platform=3.1087.0-pr-3134-a3d2; role=Administrator
    at: 2026-10-21T15:43:30.733Z
    by: session:638251b6
    bodyHash: 062af6acec2a
    who: kutasinaelena
    note: "Re-observed: collapsed state read back via GET settings/v2/me/values (isCollapsed true) and applied after sign-in in a fresh isolated browser context."
---
With the Classic Admin shell (vc-platform PR #3134 build), clicking "Collapse menu" at the bottom of the main menu collapses it to icons and sends POST /api/platform/profiles/currentuser (204), writing the UserProfile setting VirtoCommerce.Platform.UI.MainMenu.State. The collapsed state survives navigating to another workspace and a full page reload; expanding writes it back. 5 rapid toggles stayed consistent. Source (userProfile.js onMenuChanged) confirms the state is per user, not per browser.
