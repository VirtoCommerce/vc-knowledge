---
id: KB-1E89AC5E
subject: On /compare at 390 px each product column is 112 px wide and the product title link is line-clamped to 2 lines, so only a name of roughly 14 characters or fewer renders on one line
plane: experiential
question: How tall can a product name get in a /compare column header on a phone, and what does it take for it to render on one line?
questions:
  - text: Is the product title in the compare table header line-clamped on mobile?
  - text: How wide is a product column on /compare at 390 px?
  - text: Why do two different-length product names render at the same height on /compare mobile?
status: active
appliesTo:
  - axis: feature
    value: product-compare
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 390
anchors:
  - coordinate: /compare
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 6cf3e54e8335fd8d
    conditions: theme=2.63.0-pr-2635-8318; browser=edge; auth=guest
    at: 2026-11-20T12:38:36.950Z
    by: session:fcdb91ae
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa
    build: 6cf3e54e8335fd8d
    conditions: theme=2.63.0-pr-2635-8318; browser=chromium; auth=guest
    at: 2026-11-20T13:06:34.737Z
    by: session:fcdb91ae
    bodyHash: cc18fb5b77d7
    who: kutasinaelena
    note: "Chromium 390x844: 112 px columns, title link 88 px wide; 'Red hat' renders on one line (16 px high), 'White Basic Women T-shirt' on two (33 px); Add to cart tops both 505 px. The ~14-char bound is optimistic for wide glyphs: 'AKDOV Ultimate' (14 chars, partly uppercase) wrapped to two lines (33 px); 'Snack game' (10) and 'Xiaomi 17' (9) stayed on one."
---
Guest, Edge 390x844, two products in one tab (AGENT-TEST-Compare-Alpha, 24 chars without spaces; AGENT-TEST-Compare-LongName + 62-char suffix). Each th scope=col is 112 px wide; the title link has -webkit-line-clamp 2, line-height 16.38 px, rendered height 32.75 px for both (full scrollHeight 49 vs 164). The Add to cart buttons of both columns sit on the same top (505.15 px). A one-line title therefore needs a name of about 14 characters or fewer; the 11 Business laptops of Virto Start Electronics on this stand all have names of 38+ characters, so a one-line/two-line pair was not found there.
