---
id: KB-4135FFBE
subject: The VcShape Mask story requests /assets/undefined (404) because its img arg is a site-root path that the kit's image helper cannot resolve
plane: experiential
question: Why does the VcShape Mask story in Storybook request /assets/undefined?
questions:
  - text: Does the VcShape Mask story show its banner image?
  - text: What image path forms does VcShape's img prop accept?
  - text: Which request 404s on components-molecules-vcshape--mask?
status: active
appliesTo:
  - axis: component
    value: vcshape
  - axis: surface
    value: storefront-ui
  - axis: tool
    value: storybook
anchors:
  - coordinate: GET /assets/undefined
evidence:
  - method: observation
    deployment: vcst_qa
    build: 80aa52bbcc451dd4
    conditions: theme=2.62.0-pr-2620-b47e; browser=chromium
    defect: suspected
    at: 2026-11-06T11:50:11.604Z
    by: session:213be081
    who: Lenajava1
---
Chromium, no sign-in. Storybook story components-molecules-vcshape--mask, on the PR build (vc-frontend PR 2620 @ b47e1167) and on the dev build: the page sends GET <storybook>/assets/undefined -> 404 and the console logs the failed load. The shape's computed background-image is url(.../assets/undefined) while its mask-image is the resolved academic-cap SVG, so the story shows a 300px academic-cap silhouette in a flat secondary colour, no banner. The story's img arg is "/static/images/home/main-banner.webp"; VcShape resolves img through getImageUrl, a Vite new URL(`../../assets/images/${fileName}`, import.meta.url) lookup over files in client-app/assets/images, so a site-root path misses the lookup and becomes "undefined". Control: the Image story with img "main-banner.webp" loads /assets/main-banner-<hash>.webp with 200. Same story args at base dev 37cdaf67, so this predates the PR.
