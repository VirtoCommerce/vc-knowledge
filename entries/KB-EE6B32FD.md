---
id: KB-EE6B32FD
subject: On vc-frontend-next the language picker switches locale from both the desktop preferences menu and the 375px menu; the mobile list shows bare language names, so en-GB and en-US both read 'English'
plane: experiential
question: How does language switching behave in the vc-frontend-next header preferences menu and mobile menu?
status: active
appliesTo:
  - axis: feature
    value: language-selector
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /catalog
  - coordinate: /de/catalog
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T10:56:17.177Z
    by: session:df9d131f
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T11:00:44.930Z
    by: session:df9d131f
    who: Lenajava1
    note: "Re-observed on / (guest): the 'USD · EN' header button is reachable by data-test-id preferences-menu and opens the 'Currency, language and theme' dialog with Currency (9 options incl. USD, EUR, PTS), Appearance and Language (15 region-labelled locales) in one dialog; the current currency is also not exposed as selected in the accessibility tree."
---
On theme vc-frontend-next 3.0.0-alpha.2685 the desktop 'USD · EN' button opens a dialog 'Currency, language and theme' with Currency, Appearance (Light/Dark/System) and Language sections; language options are labelled with region ('English (United Kingdom)', 'English (United States)', 'Deutsch (Deutschland)'). Choosing Deutsch moved to /de/catalog and re-rendered the UI in German (button 'USD · DE'). At 375px the main-menu drawer has a flag button opening a list with flag + bare names ('polski', 'Deutsch', 'English', 'English' ...); the two English entries differ only by flag, and choosing the second English returned to the unprefixed URL in English. Neither list exposes the current language as selected in the accessibility tree (no pressed/current state); the visual selected item is bold.
