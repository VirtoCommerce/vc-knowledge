---
id: KB-FCBAAC8C
subject: An Announcement bar added in the Site Builder designer starts with no blocks and renders nothing; its settings panel's 'Text' field is the text COLOUR picker, and the text input appears only after expanding the tree row and choosing Add block -> Announcement
plane: experiential
question: How do you enter the text of an announcement bar in the Site Builder designer?
questions:
  - text: Announcement bar text field missing in Site Builder designer
  - text: Why does the Text setting of the announcement bar show colours?
  - text: Why does a new announcement bar not show in the designer preview?
  - text: Where is Add block for the announcement-bar section?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/content/site-builder-designer/{pageId}
  - coordinate: GET /api/site-builder/catalog
evidence:
  - method: observation
    deployment: vcst_dev
    build: 64e98dace3678e62
    conditions: platform=3.1101.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; theme=2.64.0-pr-2698; store=Electronics; role=administrator
    defect: suspected
    at: 2026-11-27T10:20:53.343Z
    by: session:c8a6829d
    who: ivan-kalachikov
---
Electronics, theme 2.64.0-pr-2698: designer -> HEADER -> Add section -> Announcement bar. The section is added collapsed with zero blocks, the preview shows no bar, and the settings panel shows Home page only, Bar (colour) and Text (colour token combobox, default Additional 50); typing a sentence into Text filters the colour list to 'No options'. Clicking the small Blocks chevron on the tree row reveals Add block -> Announcement; that block's panel has 'Text · en-US' (default 'Announce something here') and Link, and the preview then shows the bar with the typed text. The uploaded catalog (GET /api/site-builder/catalog) carries the same announcement-bar schema: blocks [announcement], settings home_only / background 'Bar' / color 'Text', and no default block.
