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
---
In Admin SPA Content -> store -> Pages -> Add -> Design page, typing a permalink equal to a published page's permalink shows "Other pages have the same permalink" (needs Content indexed search enabled and the other page published/indexed). Changing it to a format-invalid value (e.g. appending ?x, or an unrelated value with ?x) shows the new format error and disables Create, but the duplicate warning listing the other page stays on screen for a value that is no longer a duplicate; a valid non-duplicate value clears it. The duplicate check is an async validator, which AngularJS skips while the new sync permalinkFormat validator fails.
