---
id: KB-3A51CEC3
subject: The Admin White labeling blade requests a _64x64 thumbnail of an SVG store logo, which returns 404
plane: experiential
question: Why does opening the White labeling blade in the Admin SPA log a 404 for logo_*_64x64.svg?
questions:
  - text: Are 64x64 thumbnails generated for SVG white-labeling logos?
  - text: What 404 appears in the console when opening store White labeling in the admin?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: GET /cms-content/assets/customization/{id}
  - coordinate: GET /api/white-labeling/store/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; store=B2B-store
    at: 2026-11-27T12:30:33.967Z
    by: session:d2ee4088
    who: Lenajava1
---
On vcst_qa 2026-11-27 (platform 3.1101.0), with B2B-store's white-labeling logoUrl pointing to an .svg asset under cms-content/assets/customization/, clicking the store's 'White labeling' widget triggered GET /cms-content/assets/customization/logo_B2B-store_<n>_64x64.svg which returned 404 (console: Failed to load resource 404). The blade itself loaded normally (GET /api/white-labeling/store/B2B-store 200). The admin appears to derive a _64x64 thumbnail URL for the logo, and no such thumbnail exists for the SVG. Not checked against docs; may be expected for SVG.
