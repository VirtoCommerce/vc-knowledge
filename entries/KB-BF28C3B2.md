---
id: KB-BF28C3B2
subject: "Designer asset picker Upload in a single-asset control accepts several files: one POST /api/assets per file into the current folder, and only the last uploaded file ends up selected"
plane: experiential
question: What happens when several files are uploaded at once in the Page Builder Designer asset picker opened from a single-image control?
questions:
  - text: Can I upload multiple files from a single Image control's asset library picker?
  - text: Which asset is selected after a multi-file drop in the Designer picker?
  - text: Does the Designer asset picker send one upload request per file?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/assets
  - coordinate: page-builder-designer/index.html#/pages
evidence:
  - method: observation
    deployment: vcst_qa
    build: e68e6af5958b13d2
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-166-3a31; theme=2.61.0-pr-2570
    at: 2026-10-26T08:37:20.091Z
    by: session:153e3820
    who: kutasinaelena
    note: "distinct: KB-EDAE238D is about creating folders (POST /api/assets/folder); this is about uploading files (POST /api/assets) and which one gets selected."
---
From an Image block (theme schema type images, multiple:false) the picker's Upload file chooser accepted 3 PNGs in one pass and a real drop of 2 files on the grid uploaded both: each file is sent as its own multipart POST /api/assets?folderUrl=<current folder> (200), the grid reloads, and only the last uploaded file is selected (earlier selection cleared). Select applies one asset and the saved page content stores the image as one scalar URL string. In source, a batch containing any file larger than the control descriptor's maxFileSize shows an inline 'file too large' error and uploads nothing; no theme image control on this stand declares maxFileSize, so that path was not observable live.
