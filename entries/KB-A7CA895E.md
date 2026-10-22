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
---
In Admin SPA Content -> store -> Pages -> Add -> Design page, typing a permalink equal to a published page's permalink shows "Other pages have the same permalink" (needs Content indexed search enabled and the other page published/indexed). Changing it to a format-invalid value (e.g. appending ?x, or an unrelated value with ?x) shows the new format error and disables Create, but the duplicate warning listing the other page stays on screen for a value that is no longer a duplicate; a valid non-duplicate value clears it. The duplicate check is an async validator, which AngularJS skips while the new sync permalinkFormat validator fails.
