---
id: KB-07A9E6AE
subject: On /account/coupons a long coupon code wraps inside its copy button at 375, 1024, 1280 and 1920, but at 1024 the button is squeezed to about 99 px so a 45-character code breaks over ten lines
plane: experiential
question: Does a long coupon code stay inside its button on /account/coupons at mobile and desktop widths?
questions:
  - text: How does the account coupons page render a very long coupon code?
  - text: Does the coupon copy button overflow with a long code?
status: active
appliesTo:
  - axis: role
    value: b2b-buyer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/coupons
  - coordinate: Query.promotionCoupons
evidence:
  - method: observation
    deployment: vcst_qa
    build: fcea7ce160c179b6
    conditions: theme=vc-frontend-next-3.3.0-pr-23-7be3; store=B2B-store; browser=chromium
    at: 2026-11-16T12:58:01.831Z
    by: session:4cdfee7c
    who: Lenajava1
---
Signed-in B2B buyer on /account/coupons (vc-frontend-next 3.3.0-pr-23-7be3, carries vc-frontend 2.62.0), coupon AGENTTESTSRLONGCODEWRAPCHECK0123456789ABCDEFGH (45 chars). The code span stays inside its 'Click to copy' button at every width (scrollWidth equals clientWidth, no page overflow): 375 - button 302 px wide, two lines; 1024 - button 99 px wide and 311 px tall, the code breaks into ten lines although the card has free space between the description and the button; 1280 - 135 px, 193 px tall; 1920 - 241 px, 119 px tall.
