---
id: KB-A7CA895E
subject: Legacy PageBuilder page blade keeps a stale "Other pages have the same permalink" warning once the permalink becomes format-invalid
plane: experiential
question: Does the legacy Content Pages blade update the duplicate-permalink warning while the permalink format is invalid?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/content/search
evidence:
  - method: observation
    deployment: local
    build: 3a492aac51b809f5
    conditions: platform=3.1089.0; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-e4ff
    defect: suspected
    at: 2026-10-22T15:15:07.269Z
    by: session:2c92b249
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcst_qa
    build: 4fd52d6a347c726e
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-add2; store=B2B-store
    at: 2026-10-23T10:48:32.647Z
    by: session:562b04df
    bodyHash: 5e451cc4bfed
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: platform 3.1090.0-pr-3133-8b4a vs 3.1089.0; module:VirtoCommerce.PageBuilderModule 3.1036.0-pr-178-add2 vs 3.1036.0-pr-178-e4ff; virtocommerce.search 3.1010.0 vs 3.1011.0
    note: "Fixed on VirtoCommerce.PageBuilderModule 3.1036.0-pr-178-add2: in Content -> B2B-store -> Pages -> Add -> Design page, '/contacts' shows \"Other pages have the same permalink\" (contacts.en-US.page) with Create enabled; appending '%' hides the warning and its list, shows the format error and disables Create; Backspace brings the warning back and re-enables Create. The template now gates the warning with ng-if on !$error.permalinkFormat."
    resolved: defect-fixed
    resolvedAt: 2026-10-29T10:00:46.744Z
    resolvedBy: session:4d39d1e8
    resolvedIn: judge/batch-20261029-1343
    resolution: PR 178 commit add254f0 gates the duplicate warning with ng-if !$error.permalinkFormat in edit-page.tpl.html (released in 3.1038.0); on 3.1038.0 on vcst_qa (2026-10-29) the warning disappears while the permalink is format-invalid and returns when it is valid again.
    defect: suspected
    fixedIn: VirtoCommerce.PageBuilderModule@3.1038.0
  - method: observation
    deployment: vcptcore_qa
    build: 37f84b650ab7f9a6
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-61e8; store=B2B-store
    at: 2026-10-26T22:58:55.908Z
    by: session:573da9cc
    bodyHash: 5e451cc4bfed
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: module:VirtoCommerce.PageBuilderModule 3.1036.0-pr-178-61e8 vs 3.1036.0-pr-178-e4ff; virtocommerce.pages 3.1011.0-pr-23-4bcd vs 3.1010.0; virtocommerce.search 3.1010.0 vs 3.1011.0
    note: "On PageBuilderModule 3.1036.0-pr-178-61e8, new Design page with the permalink of a published, indexed page: 'Other pages have the same permalink' plus the matching page is shown and Create is enabled; appending '%' hides the warning and its list, shows the format error and disables Create; Backspace brings the warning back and re-enables Create. The stale warning no longer happens."
    resolved: defect-fixed
    resolvedAt: 2026-10-29T10:00:49.410Z
    resolvedBy: session:4d39d1e8
    resolvedIn: judge/batch-20261029-1343
    resolution: PR 178 commit add254f0 gates the duplicate warning with ng-if !$error.permalinkFormat in edit-page.tpl.html (released in 3.1038.0); on 3.1038.0 on vcst_qa (2026-10-29) the warning disappears while the permalink is format-invalid and returns when it is valid again.
    defect: suspected
    fixedIn: VirtoCommerce.PageBuilderModule@3.1038.0
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1090.0; module:VirtoCommerce.PageBuilderModule=3.1038.0; store=B2B-store; role=administrator; db=SqlServer
    at: 2026-10-29T10:00:44.131Z
    by: session:4d39d1e8
    note: "2026-10-29, Admin SPA Content > B2B-store > Pages > Add > Design page: '/contacts' showed 'Other pages have the same permalink' (contacts.en-US.page) with Create enabled; appending '%' (and '?x') hid the warning at once, showed the format error and disabled Create; Backspace brought the warning back. Nothing was created."
    investigation: true
    bodyHash: 5e451cc4bfed
---
In Admin SPA Content -> store -> Pages -> Add -> Design page, typing a permalink equal to a published page's permalink shows "Other pages have the same permalink" (needs Content indexed search enabled and the other page published/indexed). Changing it to a format-invalid value (e.g. appending ?x, or an unrelated value with ?x) shows the new format error and disables Create, but the duplicate warning listing the other page stays on screen for a value that is no longer a duplicate; a valid non-duplicate value clears it. The duplicate check is an async validator, which AngularJS skips while the new sync permalinkFormat validator fails.
