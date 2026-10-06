---
id: KB-F3D79CBD
subject: POST /api/assets/folder validates only the name; duplicates return 204, parentUrl is not validated and missing intermediate folders are created
plane: experiential
question: What does POST /api/assets/folder do with a duplicate folder name, an unvalidated parentUrl, or a padded name?
status: active
appliesTo:
  - axis: provider
    value: filesystemassets
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/assets/folder
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-06T17:06:47.272Z
    by: session:122efeda
    who: kutasinaelena
---
On the file-system assets provider, POST /api/assets/folder {name,parentUrl} validates `name` only (3-63 chars, lowercase letters/digits/spaces/single dashes, no leading/trailing/consecutive dashes, not empty; 400 with BlobFolderValidator messages). Creating a folder whose name already exists returns 204 with no error and leaves existing contents intact, so the Admin Asset Library workspace and the Page Builder Designer picker show no duplicate error. parentUrl is not validated: a parentUrl with segments that do not exist (even ones containing {{ }} braces) returns 204 and creates every missing intermediate folder. A name with leading/trailing spaces is accepted and stored untrimmed (the Admin and Designer UIs trim before sending).
