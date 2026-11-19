---
id: KB-BADF305D
subject: "Site Builder Vue designer: clicking a heading in the preview edits it in place and one Undo restores it; hover outlines a section and a click selects it in the section list; markdown body text is not inline-editable"
plane: experiential
question: How does inline editing in the Site Builder Vue designer preview behave, and does Undo revert it?
questions:
  - text: Does hovering a section in the Site Builder designer preview outline it?
  - text: Does clicking a section in the designer preview select it in the section list?
  - text: Can the markdown content of a Text section be edited inline in the Site Builder preview?
  - text: Does Undo in the Site Builder designer revert an inline text edit?
status: active
appliesTo:
  - axis: feature
    value: site-builder-designer
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/site-builder-shell#/site-builder-draft/site-builder-designer
  - coordinate: POST /api/site-builder/designer/save
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; theme=2.63.0-pr-2676; platform=3.1099.0; browser=firefox
    at: 2026-11-19T13:32:52.414Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: browser=edge; theme=2.63.0-pr-2676; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0
    at: 2026-11-19T18:27:11.330Z
    by: session:51ac2fe3
    bodyHash: 36fba8c5c4d8
    who: Lenajava1
    note: "Database page, Text section title: the click set contenteditable=plaintext-only on the h2, each keystroke posted {type:edit, itemId, path:[title], value, cultureName:en-US} to the designer, the panel Title followed; one Undo reverted all 7 keystrokes. In this run the \"Has unsaved changes\" alert was still shown after that Undo (Save state not read). A section of type text-block (no schema) offers no inline edit (\"Nothing to set here\"). The published storefront page source and DOM had no data-pb-*/contenteditable before or after."
---
On a file page of a store whose preview loads, added a Title section (Title field set in the panel). Hovering a section in the preview draws a dashed outline with the section name tag and highlights its row in the section list; clicking a section box selects it (solid outline, row highlighted, settings panel opens for it). Clicking the rendered heading also puts an edit frame on it; typing appended text in place, the panel's Title field followed, "Has unsaved changes" appeared and Undo enabled. One Undo click restored the heading and the field to the saved value, Save went back to disabled and Redo enabled. Clicking the Text section's rendered markdown ("Hello, markdown!") opens no inline editor: that content is edited only in the panel. Section add / drag-reorder / Hide-Show / Copy + Paste / Remove (with a "Remove "Title" from the page?" confirmation) each update the preview immediately. Note: the preview has a pointer-capturing overlay, so automation must use coordinate mouse input.
