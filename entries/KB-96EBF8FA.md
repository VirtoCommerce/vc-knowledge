---
id: KB-96EBF8FA
subject: "When the AI assistant panel closes, the Site Builder designer re-reads the open page: a server-side change reloads it when there are no unsaved edits, and with unsaved edits keeps them and warns that the assistant changed the page; the panel's own Close (X) cannot be clicked"
plane: experiential
question: What does the Site Builder designer do with the open page when the AI assistant panel closes after the page was changed on the server?
questions:
  - text: Does closing the Virto OZ panel reload the designer page?
  - text: Are unsaved designer edits kept when the assistant changed the page?
  - text: Can the AI assistant panel be closed with its X button?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/site-builder-shell
  - coordinate: POST /api/site-builder-pages/grouped/{id}/content-json
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; platform=3.1099.0; browser=edge; viewport=1920x1080
    at: 2026-11-19T18:27:22.971Z
    by: session:51ac2fe3
    who: Lenajava1
---
Admin, database page in /apps/site-builder-shell designer, OzAgentUrl set. The Virto OZ panel itself showed only "Login to access the app" (no control, GET /user 401), so the assistant's own save and RELOAD_BLADE were not exercised; its write was stood in for by POST /api/site-builder-pages/grouped/{groupId}/content-json (the assistant's save tool) while the panel was open. No unsaved edits: closing the panel with the toolbar AI Assistant toggle reloaded the preview with the server title. With an unsaved Title edit: after close the local edit stayed in the preview and the panel, "Has unsaved changes" stayed, and a notification read "The AI assistant changed this page. Your unsaved edits are kept: copy what you need, then reopen the page to see its changes." The panel header's Close (X) is drawn but the workspace <main> sits over it (elementFromPoint returns MAIN.vc-app__workspace), so a real mouse click on it does nothing at 1920x1080; the toolbar toggle closes it.
