---
id: KB-93FFCC74
subject: Storefront return wizard rejects a disallowed attachment extension client-side, without calling the upload endpoint
plane: experiential
question: What happens on /account/returns/{id}/edit when the buyer attaches a .xlsx file?
questions:
  - text: Can a buyer attach an Excel file as a photo when returning products?
  - text: /account/returns/{id}/edit .xlsx 'File format is not allowed' return-attachments
  - text: Is a disallowed return attachment rejected client-side or by /api/files/return-attachments?
  - text: Which file types and size limit are allowed for return wizard attachments?
concepts:
  - id: return
  - id: asset
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/returns/{}/edit
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T19:55:54.700Z
    by: session:0cfc9f97
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T19:56:02.654Z
    by: session:0cfc9f97
    who: kutasinaelena
---
On theme 2.59.0-pr-2477, attaching Test.xlsx in the return wizard (/account/returns/{id}/edit) lists the file with 'File format is not allowed', shows 'Fix file upload errors to continue.' and 'Some files failed to upload. Remove or retry them before submitting.', keeps Submit return disabled and still counts the line under 'line(s) still need a photo'. No request to /api/files/return-attachments is issued; the allowed set shown is JPG, JPEG, PNG, WEBP, PDF, 5MB, max 5 files.
