---
id: KB-B6BCD705
subject: "Site Builder history restore refuses with 400 a theme: item outside the store's theme folder, before anything is written; the theme-folder path restores the version as a draft and records a new version"
plane: experiential
question: What does POST /api/site-builder/history/{versionId}/restore answer for a theme item whose path is outside the store's theme folder?
questions:
  - text: Can a Site Builder history restore write a theme file outside the store's theme folder?
  - text: Does MCP restore_version check that a theme path is in the store's theme folder?
  - text: What happens when a restore names theme:custom-sections/<t>.json without the theme folder?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder/history/{id}/restore
  - coordinate: GET /api/site-builder/history
evidence:
  - method: observation
    deployment: vcst_dev
    build: 1b6e861ce41cd6cb
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-6088; store=Electronics; role=administrator; setting:history=enabled (GitHub App)
    at: 2026-11-26T13:27:00.468Z
    by: session:c8a6829d
    who: ivan-kalachikov
---
On Electronics (theme folder default, history enabled through the GitHub App) a custom section type was saved twice, giving two history versions. Restoring the older version with item theme:custom-sections/<t>.json, theme:Default/custom-sections/<t>.json or theme:no-such-theme/custom-sections/<t>.json each answered 400 {"error":"\"<path>\" is not in the store's theme folder \"default\"."}; the draft's content and eTag, the version count and every listing under the rejected paths stayed unchanged. The same restore with theme:default/custom-sections/<t>.json answered 200 {saveId}, brought the older content back as the draft byte for byte and added one version "Restored <id>". MCP restore_version behaves the same: a dry run and a real call with a path outside the folder return isError naming the folder; with the theme folder the dry run lists the field changes and the real call restores. The comparison is case-sensitive (Default/ is refused).
