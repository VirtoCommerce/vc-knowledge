---
id: KB-0B4DAD09
subject: Platform backup is AES-encrypted by default; restore of it needs the one-time password from the export response, or export with passwordProtect:false
plane: experiential
question: Why does POST /api/platform/import fail with "Invalid backup password / No password available for AES encrypted stream", and how is the backup password supplied?
questions:
  - text: How do I script a platform backup and restore round trip?
  - text: Where is the platform backup password returned?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.backuprestore
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/platform/export
  - coordinate: POST /api/platform/import
evidence:
  - method: observation
    deployment: local
    build: 3a492aac51b809f5
    conditions: platform=3.1089.0
    at: 2026-10-22T15:15:07.172Z
    by: session:2c92b249
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcst_qa
    build: 4fd52d6a347c726e
    conditions: platform=3.1090.0-pr-3133-8b4a
    at: 2026-10-23T10:23:52.458Z
    by: session:562b04df
    bodyHash: 5e460d5d996c
    who: Aleksandra-Mitricheva
    note: Export with passwordProtect:false returned a notification only (no password), entries deflate-compressed and unencrypted; after a clean restore of an uploaded backup the blob was gone from backups/.
---
POST /api/platform/export {modules, exportManifest} returns {notification, password}: by default (passwordProtect=true) the zip entries except Manifest.json are AES-encrypted (zip method 99) and the plaintext password is in that HTTP response only. Restoring it with POST /api/platform/import {fileUrl, exportManifest, modules} and no password finishes with errorCount 1 "Invalid backup password\nNo password available for AES encrypted stream" and imports nothing. Sending passwordProtect:false on export produces an unencrypted zip (manifest isEncrypted:false) that restores with 0 errors; the import body field for an encrypted backup is "password". A fully clean restore deletes the uploaded backup blob from backups/; a failed one keeps it.
