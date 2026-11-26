---
id: KB-EFD3AE1F
subject: On /account/coupons a coupon code with a long unbroken segment overflows the dashed code button between 1024 and 1280 px viewport width
plane: experiential
question: Does a long coupon code stay inside its dashed code button on the coupon card of /account/coupons at 1024-1280 px?
status: active
appliesTo:
  - axis: component
    value: couponitem regular layout
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 1024-1280
anchors:
  - coordinate: /account/coupons
evidence:
  - method: observation
    deployment: mac_local
    conditions: platform=3.1087.0; theme=2.61.0; browser=chromium-mobile-emulation; role=Sales Representative
    defect: suspected
    at: 2026-10-21T19:35:26.800Z
    by: session:6eb5c164
---
Signed in, /account/coupons, regular CouponItem card, 12-column grid from the lg breakpoint (1024 px) with the code in a col-span-2 cell. I swept the viewport width with the page loaded and measured the glyph extent of the code (a Range over the code span) against the button border. The code AGENT-TEST-P6-1792609407240-1235BB47 (36 characters, a 13-digit segment that cannot break) paints past the right border of the button by 36.4 px at 1024, 23.7 px at 1100, 7.1 px at 1200 and 0.4 px at 1280 (11 px into the padding that the content box excludes). At 1366 and wider, and at 1023 and narrower, it stays inside. The button is 101 px wide at 1024, 130 at 1200 and 137 at 1280. No horizontal scrollbar appears; the text just crosses the dashed border. Observed on the unchanged host card; the same card renders on the Sales Rep customer coupons page with identical numbers.
