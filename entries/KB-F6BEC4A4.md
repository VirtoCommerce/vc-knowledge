---
id: KB-F6BEC4A4
subject: On the PDP image gallery (desktop), the thumbnail prev/next buttons are hidden through Swiper's lock class when every thumbnail fits (4 or fewer), and shown with loop when there are 5 or more; with 5 images the looped strip pads to 8 slides with 3 empty ones, so Previous from the first slide shows image 5 next to 3 empty thumbnail cells
plane: experiential
question: When does the product page image gallery show its thumbnail prev/next buttons, and how does the thumbnail strip loop?
questions:
  - text: Are the PDP gallery thumbnail arrows hidden for a product with 4 images?
  - text: Why does the product gallery show empty thumbnail cells after clicking Previous?
  - text: Does the image gallery thumbnail strip loop for products with 5 or more images?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: desktop
anchors:
  - coordinate: /product/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 80aa52bbcc451dd4
    conditions: theme=2.62.0-pr-2620-b47e; setting:design=classic; store=B2B-store; browser=chromium
    defect: suspected
    at: 2026-11-06T10:58:28.489Z
    by: session:213be081
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 80aa52bbcc451dd4
    conditions: theme=2.62.0-pr-2620-b47e; platform=3.1094.0; store=B2B-store; browser=chromium; viewport=1280x900; role=corporate pool user (signed in)
    at: 2026-11-06T11:49:51.142Z
    by: session:213be081
    bodyHash: 0eaea3d6f4f4
    defect: suspected
    who: Lenajava1
    note: "Re-observed signed in (not only anonymous). After one Previous click the 8 thumbs slides read: data-swiper-slide-index 4 (image 5) then 5,6,7 visible in the strip; 5-7 carry Swiper's class swiper-slide-blank and no img. Mechanism: thumbs Swiper loop + slidesPerGroup 4 with loopAddBlankSlides at its default (true) pads 5 slides to 8."
---
vcst_qa, theme 2.62.0-pr-2620-b47e (vc-frontend PR 2620, which carries VCST-6529), Chromium 1280 desktop, anonymous. Product with 4 images: both .image-gallery__nav buttons carry swiper-button-disabled + image-gallery__nav--locked and compute display:none from the first rendered frame (rAF probe during an SPA navigation saw no visible frame, so no layout shift). Product with 5 images (Sony XBR-49X830C): both buttons display:flex 28x28, not disabled; the thumbs Swiper (slidesPerView 4, slidesPerGroup 4, loop) renders 8 slides (data-swiper-slide-index 0-7), slides 5-7 empty (no img). Clicking Previous from the start wraps to the end: the visible strip is image 5 followed by 3 empty cells. The loop/slidesPerGroup config is unchanged from the base dev 37cdaf67, so the blank-cell behaviour predates the PR.
