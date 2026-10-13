---
id: KB-D0AF91D1
subject: The home-page slider and features blocks have no alt-text field, so their images render without alt whatever the content says
plane: experiential
question: Is the missing alt on the storefront home-page slider and features images a content gap an admin can fill, or a theme defect?
status: active
appliesTo:
  - axis: page
    value: home
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.page
  - coordinate: /homepage.page
evidence:
  - method: observation + source read (vc-frontend dev section schemas and components)
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2543-0127-01276486; store=B2B-store; auth=guest; viewport=1920
    at: 2026-10-13T17:21:02.415Z
    by: session:aa76d86f
    who: Lenajava1
---
The storefront home page rendered its blocks from the Content-module page served by GraphQL Query.page (id B2B-store::pages::/homepage.page). In that content, slider slides carry only image, title and text, and features columns carry only image, title and text. The theme's section schemas offer the same fields and no alt field. The sibling image block offers 'alttext' and call-to-action-with-image offers 'imageDescription'. The features component renders VcImage with src only, even though each column has a filled title, so the img has no alt attribute. The slider binds aria-label to the slide title with a nullish fallback, so an empty-string title yields aria-label="". The 6 imgs fail axe image-alt (critical), 3 of 3 reloads. An admin cannot fix this by editing content; it is a theme code/schema defect.
