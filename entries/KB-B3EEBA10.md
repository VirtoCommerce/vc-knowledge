---
id: KB-B3EEBA10
subject: Admin SPA grid boolean cells (`i.table-ico.fa.fa-check`) are icon-only, painted #a6a6a6 (about 2.2-2.4:1), and expose nothing to the accessibility tree
plane: experiential
question: How are boolean flags rendered in Admin SPA ui-grid cells and are they accessible (contrast, text alternative)?
status: active
appliesTo:
  - axis: layer
    value: platform-grid-style
  - axis: surface
    value: admin-spa
  - axis: wcag
    value: 1.4.11-1.1.1
anchors:
  - coordinate: /workspace/store
evidence:
  - method: observation (getComputedStyle + a11y tree + contrast math)
    deployment: vcptcore_dev
    at: 2026-10-05T21:29:53.259Z
    by: session:f9a8159e
---
On a dev stand (Platform 3.1077.0-alpha), a ui-grid boolean column rendered with `<i class="table-ico fa fa-check" ng-if="COL_FIELD">` shows a 28 px Font Awesome check glyph with color rgb(166,166,166) and a 1px white text-shadow, and nothing at all when false. Measured contrast of the glyph: 2.43:1 on a white row, 2.31-2.39:1 on the striped rows (#f9f9f9/#fdfdfd), 2.23:1 on the hover row (#ecf7fc), all under the WCAG 1.4.11 non-text 3:1 minimum. The `<i>` has no aria-label, title, aria-hidden or visually hidden text; in the accessibility tree the gridcell has an empty name, so the row's name does not mention the flag and true is indistinguishable from false for assistive technology. This is the platform's default `.table-ico` style, shared by any module grid that uses it. Tooling note: the repo's `nonTextContrastAuditSnippet` resolves stroke, then fill, then color; on an HTML icon-font element `fill` computes to rgb(0,0,0), so it mis-reads these glyphs as black (false 19:1 pass for the check, false 2.02:1 failure for the white blade-nav icons) - read the glyph colour from CSS `color` (or the ::before pseudo-element) for non-SVG icons.
