---
id: KB-B4DC77F0
subject: Page Builder Designer needs platform:setting:read for a non-admin, else preview path is null and blocks are not editable
plane: experiential
question: Which permissions does a non-admin need to edit a page in the Page Builder Designer?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-page-builder-designer
anchors:
  - coordinate: GET /api/platform/settings/VirtoCommerce.PageBuilderModule.General.StorePreviewPath
  - coordinate: POST /connect/token
  - coordinate: POST /api/page-builder-pages/grouped/{groupId}/content
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-06T20:45:01.836Z
    by: session:122efeda
    who: kutasinaelena
---
A Manager-type user with store:access, store:read, builder:access, builder:read, builder:update, platform:asset:access, platform:asset:read could open the Designer but GET /api/platform/settings/VirtoCommerce.PageBuilderModule.General.StorePreviewPath returned 403, previewPath resolved to null, the preview showed the storefront home page and the block buttons in the left tree were disabled. Adding platform:setting:read made previewPath /designer-preview, the page preview loaded and blocks became editable; Select from asset library and Save (POST /api/page-builder-pages/grouped/{id}/content 204) then worked. Separately, for these restricted users the Designer's cookie-to-bearer exchange (POST /connect/token) redirects to /Account/AccessDenied (404) and the Designer shows a "Session expired" sign-in dialog.
