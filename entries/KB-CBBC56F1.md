---
id: KB-CBBC56F1
subject: Clone on a language-version blade opened from the Multilanguage Page blade creates a standalone Landing copy but throws an unhandled '[BladeMessaging] Caller blade … not found in stack' rejection
plane: experiential
question: Does Clone from a Multilanguage language-version blade raise a console error?
questions:
  - text: What does Clone do on a language version of a Multilanguage Page?
  - text: Why does BladeMessaging Caller blade not found appear in Page Builder?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/page-builder-shell
  - coordinate: POST /api/page-builder-pages/grouped
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-175-a3bc; framework=@vc-shell/framework 2.6.0; browser=msedge
    defect: suspected
    at: 2026-10-23T13:00:25.757Z
    by: session:0204f22f
    who: Aleksandra-Mitricheva
---
Multilanguage Page blade -> language row 'Open' -> version blade (#/page-builder-draft/details/{groupId}) -> Clone: a standalone Draft Landing page '<name> (copy)' / '<permalink>-copy' in the version's culture is created (multilanguagePageId null) and opened, and the console logs '[@vc-shell/framework#global-error-handler] Unhandled rejection: Error: [BladeMessaging] Caller blade 'blade_2_…' not found in stack' from the reload callParent. Opening the same version blade directly by URL and cloning, or cloning a plain Landing page opened from the pages list, logs no error. No visible error toast.
