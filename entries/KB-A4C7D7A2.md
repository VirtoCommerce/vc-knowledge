---
id: KB-A4C7D7A2
subject: Admin SPA ui-grid blades carry three axe-core critical violations as a platform baseline (aria-required-parent, aria-required-children, button-name on blade-nav maximize/close), independent of any module change
plane: experiential
question: What does axe-core report on an Admin SPA blade that hosts a ui-grid, and is it caused by the module that owns the blade?
status: active
appliesTo:
  - axis: layer
    value: platform-blade-chrome
  - axis: scanner
    value: axe-core-4.10.2
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: /workspace/store
evidence:
  - method: observation (axe.run scoped per blade + real Tab walk + computed styles)
    deployment: vcptcore_dev
    at: 2026-10-05T21:29:52.998Z
    by: session:f9a8159e
---
axe-core 4.10.2 (tags wcag2a, wcag2aa, wcag21a, wcag21aa, wcag22a, wcag22aa) loads in the Admin SPA via a script tag from cdnjs (no CSP block) and runs fine when scoped to one blade. On a dev stand (Platform 3.1077.0-alpha) the untouched platform Stores list blade and a module blade that only adds a ui-grid (Debug SEO links Stages/Candidates) report the same rule ids: aria-required-children (critical; the role=grid wrapper contains the Grid Menu role=button), aria-required-parent (critical; one per column header, role=columnheader without a row parent), button-name (critical; the blade-nav maximize and close buttons contain `<i aria-hidden="true" title="Maximize|Close">` so the buttons have no accessible name). The Stores blade additionally reports aria-command-name and a color-contrast violation. Grid text gets color-contrast "incomplete" (not violation) because the platform styles it with text-shadow: 1px 1px 0 #fff, so axe cannot decide; computed contrast of grid text (#333 on #f9f9f9/white) is ~11.6-12:1. Real keyboard Tab focus on blade-nav buttons, the Grid Menu and the column header buttons resolves outline-style: none and box-shadow: none while :focus-visible matches (no visible focus indicator), and the Grid Menu icon container is 24x22.4 px overlapping the last column header button. So a module blade that only adds a ui-grid inherits these; they belong to platform chrome and should be recorded once against the platform, not per module blade.
