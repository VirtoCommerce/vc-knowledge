---
id: KB-EC50B8C0
subject: Site Builder shell page blade shows one success toast for Save, Publish, Unpublish, Archive, Clone, Save content and Load content
plane: experiential
question: Which success toasts does the Site Builder shell page details blade show after Save, Publish, Unpublish, Archive, Clone, Save content and Load content?
questions:
  - text: Does Site Builder show 'Page published successfully' after Publish?
  - text: Is there a toast after archiving a page in the Site Builder shell?
  - text: What notification appears after Unpublish in /apps/site-builder-shell?
  - text: What is the Save content download file name in the Site Builder shell?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/site-builder-shell
  - coordinate: POST /api/site-builder-pages/grouped/publishing/{id}
  - coordinate: POST /api/site-builder-pages/grouped/archive
  - coordinate: GET /api/site-builder-pages/grouped/{id}/content
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; vc-shell=2.7.0; store=B2B-store; role=admin
    at: 2026-11-19T20:12:04.793Z
    by: session:07a3812a
    who: Lenajava1
---
As administrator in /apps/site-builder-shell (B2B-store), page details blade, on own AGENT-TEST pages: Save -> PUT /api/site-builder-pages/grouped 200 and toast 'Page saved successfully'; Publish -> POST /grouped/publishing/{id}?publish=true 200 and 'Page published successfully'; Unpublish (no confirmation) -> ?publish=false 200 and 'Page unpublished successfully', status chip returns to Draft; Archive -> Confirmation 'Are you sure you want to archive this page?' -> Confirm -> POST /grouped/archive?ids= 204, blade closes, 'Page archived successfully'; Clone -> 'Page cloned successfully' and the blade swaps to '<name> (copy)' with permalink '<permalink>-copy' (a second clone gets '(copy 2)' / '-copy-2'); Save content -> GET /grouped/{id}/content?draft=true 200, a download named '<page name>.json' (object with name, permalink, cultureName, visibility, userGroups, organizationId, startDate, endDate, content:{settings, content[]}) and 'Content saved to file successfully'; Load content (Draft list only) -> 'Content loaded successfully'. Each toast is a role=status item with 'Dismiss notification' and is gone within ~5 s. Unlike the released Page Builder shell (KB-B2E4D4A5), Publish/Unpublish/Archive do show a toast here.
