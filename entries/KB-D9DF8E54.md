---
id: KB-D9DF8E54
subject: The storefront serves its index HTML with cache-control max-age=7200, so after a theme deploy a returning browser keeps loading the previous bundle for up to 2 hours
plane: experiential
question: Why does a storefront still show the old theme build after a successful theme deploy?
status: active
appliesTo:
  - axis: layer
    value: deployment
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T11:58:52.512Z
    by: session:238f2094
    who: kutasinaelena
---
On vcptcore_qa1 (2026-10-01) right after a theme update, GET /sign-in in a browser that had visited before loaded the old bundle (footer showed the previous build string, old index-*.js), while the same URL with a fresh query string loaded the new bundle and footer. The HTML response carries cache-control: max-age=7200 (Cloudflare cf-cache-status DYNAMIC), so the browser itself reuses the cached index HTML for 2 hours. Verify a new theme with a cache-busted URL or by the served asset hash, not by a plain revisit.
