---
id: KB-8F2FDFD2
subject: On theme 3.0.0-alpha.2685 in Dark appearance, the Sales Rep dashboard Edit layout bar text is legible and the hidden-stats drop zone shows a subtle two-tone diagonal stripe, not a solid block
plane: experiential
question: In dark mode, is the /company/dashboard Edit layout chrome (edit bar text, hidden-stats striped zone, keyboard-grabbed card) legible on the 3.0 theme?
status: active
appliesTo:
  - axis: appearance
    value: dark
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: vc-frontend-next-3.0.0-alpha
anchors:
  - coordinate: /company/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T11:03:38.518Z
    by: session:df9d131f
    who: Lenajava1
---
Theme 3.0.0-alpha.2685, 1920px, signed in as a sales rep. Appearance switched to Dark via the header currency/language/theme menu (default is System; options Light/Dark/System). On /company/dashboard, Edit layout shows an edit bar with Reset / Cancel / Save layout. Screenshot pixel sampling: edit bar background #2f1f17; 'Editing layout' heading and description text #f1a686 (7.93:1); keyboard-hint line brightest #948a83 (~4.68:1 at CSS scale, antialiased). Hidden-stats zone alternates #1e1814 / #261d19 in ~50/50 diagonal stripes (1.06:1 pair contrast) — visible, subtle, not solid. Its hint 'Drag a stat here to hide it' samples #4e4540 on the stripe, i.e. low contrast. Focusing a stat card and pressing Space puts it in a grabbed state (aria-pressed, live region announces 'grabbed. Position 1 of 6'), with a blue-grey outline (#637e99) and no hard black halo: the darkest pixel under the card is #1d1814 against a #1e1814 page. Escape cancels the grab, Cancel leaves edit mode, and no console errors appeared.
