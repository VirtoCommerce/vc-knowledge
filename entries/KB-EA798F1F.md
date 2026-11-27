---
id: KB-EA798F1F
subject: Site Builder theme publish also checks header sections' color pairs against the dark palette and refuses an announcement bar whose text is unreadable there
plane: experiential
question: Does POST theme/publish refuse an announcement bar whose background and text tokens fail WCAG AA in the dark palette?
questions:
  - text: Why does Publish theme fail after generating a new palette when the header has an announcement bar?
  - text: Are announcement bar colors validated for contrast in both light and dark palettes?
  - text: Which text token stays dark in both palettes for an orange announcement bar?
status: active
appliesTo:
  - axis: surface
    value: admin-designer
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/site-builder/theme/publish
  - coordinate: POST /api/site-builder/designer/save
evidence:
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; store=B2B-store
    at: 2026-11-27T00:29:05.205Z
    by: session:26322a1f
    who: ivan-kalachikov
    note: "distinct: KB-1EE973A2 is about a palette's own token pair (body-text on body-bg); this is about a header section's chosen tokens being re-checked in the dark palette."
---
B2B-store: the header holds an announcement bar with background color_warning_500 and text color_neutral_900. After Generate from colors (#0E7490 / #155E75), POST /api/site-builder/theme/publish answered 400. The designer toast said "The theme couldn't be published" and named the section: "sections/header-group.json: 'announcementbarZNn1' has a contrast of 1.69:1 in the dark palette (color_neutral_900 #e7e7e7 on color_warning_500 #fc9e00), less than the 4.5:1 WCAG 2.2 AA asks for". The light palette passed, and nothing was published. Switching the text to color_additional_950 failed the same way: 1.75:1, because additional_950 is #eceaef in the dark palette. The text token color_accent_950, dark in both palettes, published, and the theme moved to the next publication. A section's color tokens are judged in BOTH palettes at publish, and the scale tokens neutral/additional invert in the dark palette, while warning_500 does not.
