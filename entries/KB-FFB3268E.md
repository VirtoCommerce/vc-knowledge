---
id: KB-FFB3268E
subject: "Admin notification layout editor: image drop/paste/toolbar each upload once to cms-content/layouts/assets"
plane: experiential
question: Where does an image dropped or pasted into the Admin notification layout markdown editor (vcUkHtmleditor) get uploaded, and what is inserted?
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/assets
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-28T13:43:30.870Z
    by: session:p13148
    who: kutasinaelena
---
In the Admin SPA notification layout blade, dropping an image file on the vc-uk-htmleditor code pane, pasting an image, or using the toolbar Image button each fires exactly one POST /api/assets?folderUrl=cms-content/layouts/assets and inserts one markdown image link ![name](url) into the template. Pasted images are renamed image_<yyyy>-<month>-<getDay()>_<h>-<m>-<s>.png, where the day part is the weekday number (JS getDay), not the day of month. The editor binds dragenter/dragover/drop/paste jQuery handlers on its own .CodeMirror element.
