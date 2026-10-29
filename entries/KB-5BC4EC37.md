---
id: KB-5BC4EC37
subject: On /company/info, picking a non-JPG/PNG file in the Company logo picker shows "File format is not allowed", sends no upload, and throws a hidden TypeError (reading 'url')
plane: experiential
question: What happens when an organization maintainer picks a file of a disallowed type in the Company logo picker on /company/info?
questions:
  - text: Does the company logo uploader reject a .txt file client-side?
  - text: Does a rejected logo file throw a browser exception on /company/info?
status: active
appliesTo:
  - axis: role
    value: organization-maintainer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/info
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; role=Organization maintainer
    defect: VCST-6288
    at: 2026-10-29T13:26:41.183Z
    by: session:0a5c9007
    who: Lenajava1
---
Signed in as an Organization maintainer, opened /company/info (no logo set), clicked "Browse your files" under Company logo and picked a 13-byte .txt file. The block showed the inline message "File format is not allowed"; no upload request and no GraphQL mutation was sent (only a read-only GetPushMessages query followed), and the page stayed unchanged. App Insights recorded a browser exception "TypeError: Cannot read properties of undefined (reading 'url')" at the moment of the pick; browser_console_messages showed 0 errors, because the App Insights Vue plugin's errorHandler swallows it (same mechanism as KB-2DD1A73B). fileUploadOptions for the scope returned allowedExtensions [.jpg,.png] and maxFileSize 5000000. A PR build of the theme (vc-frontend PR #2612) showed the same message with no exception.
