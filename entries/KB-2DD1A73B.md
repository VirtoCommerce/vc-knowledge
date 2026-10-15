---
id: KB-2DD1A73B
subject: /company/info throws TypeError (reading 'status') and vue-i18n SyntaxError 26 on render for an organization editor, and the Vue error handler hides both from the browser console
plane: experiential
question: Does opening /company/info as an organization maintainer produce browser exceptions, and where can they be seen?
status: active
appliesTo:
  - axis: role
    value: organization-editor
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/info
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579
    defect: suspected
    at: 2026-10-15T11:10:54.596Z
    by: session:10db9d7d
    who: Lenajava1
---
Signed in as an Organization maintainer (can edit the organization), opening /company/info twice produced, each time, an App Insights browser exception TypeError "Cannot read properties of undefined (reading 'status')" (computed files.value[0].status === 'uploading' read while the logo uploader file list is empty) and vue-i18n "SyntaxError: 26" (useI18n called inside a computed that re-runs; one more fired on Logout from the page). A purchasing agent without edit rights produced none (the logo widget is editor-only). The page still renders, the logo widget shows an enabled "Browse your files" button, and no user-visible impact was seen. browser_console_messages showed 0 errors because the app's App Insights Vue plugin installs its own app.config.errorHandler, so a console-based no-errors assertion cannot catch these; they are recurring on vcst since 2026-09-18 (App Insights, vcst-qa-storefront). All /graphql calls returned 200.
