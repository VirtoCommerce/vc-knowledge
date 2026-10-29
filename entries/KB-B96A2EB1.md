---
id: KB-B96A2EB1
subject: The Page Builder shell UI language is set in the shell's own user menu and does not follow the platform user profile language
plane: experiential
question: How do you switch the Page Builder shell to another UI language?
questions:
  - text: Does the page builder shell follow the admin profile language?
  - text: Where is the language switcher of the page builder shell?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/page-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fd52d6a347c726e
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-add2; platform=3.1090.0-pr-3133-8b4a; role=administrator
    at: 2026-10-23T11:04:27.280Z
    by: session:562b04df
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    build: ed1246f57f06adbd
    conditions: module:VirtoCommerce.PageBuilderModule=3.1041.0-pr-179-5e6c
    at: 2026-10-29T13:48:54.775Z
    by: session:9b657bc7
    bodyHash: 9bdd35361a95
    who: Aleksandra-Mitricheva
    note: Shell user menu -> Language -> Deutsch relocalised blade labels immediately; user-menu items (Theme, Language, Change password, Log Out) stayed English.
---
Setting the platform Admin profile Language to Russian (Manage profile) localised the AngularJS Admin SPA, but /apps/page-builder-shell loaded afterwards stayed English. The shell has its own switcher: user button (admin Administrator) -> Language -> list of locales; picking Russian/Japanese relocalises blade labels and validation messages immediately. A field error already shown keeps the previous locale's text until the field is revalidated. The shell user-menu items (Theme, Language, Change password, Log Out) and the datepicker placeholder 'Select date and time' remained English in ru/ja.
