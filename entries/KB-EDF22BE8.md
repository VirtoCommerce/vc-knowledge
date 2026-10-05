---
id: KB-EDF22BE8
subject: "Shared Components workspace: Escape closes Component details and Rename dialog"
plane: experiential
question: "page-builder-shared-components Component details Rename: does Escape close them and where does focus go?"
questions:
  - text: Can I close the shared component details panel with the Escape key?
  - text: page-builder-shared-components Escape Component details Rename dialog focus
  - text: Where does focus go after Escape cancels the Rename shared component dialog?
  - text: At 375px does a floating button overlap the Rename button in the Shared Components workspace?
concepts:
  - id: accessibility
  - id: content-page
  - id: mobile-layout
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /page-builder-shell/page-builder-shared-components
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-01T16:19:47.205Z
    by: session:356969ae
    who: Aleksandra-Mitricheva
---
In the Page Builder shell Shared Components workspace (#/page-builder-shared-components), Escape closes the Component details side panel and focus returns to the selected grid row; Escape on the 'Rename shared component' dialog cancels it and focus returns to the Rename button. At 375px the blade toolbar renders as a floating button bottom-right; with Component details open the Refresh FAB is hidden but the AI Assistant FAB still overlays the right part of the full-width Rename button. Observed on PageBuilderModule 3.1030.0-pr-165-1ab4.
