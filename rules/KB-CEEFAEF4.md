---
id: KB-CEEFAEF4
subject: BL-SEO-002 Deleted product returns proper HTTP status
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: Query.slugInfo
evidence:
  - method: observation
    at: 2026-09-17T15:21:17.737Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SEO-002: Deleted product returns proper HTTP status `[P2-ux]`

- **Rule:** When a product is deleted, its slug returns an empty `slugInfo` from xAPI and the Vue-SPA storefront renders its client-side NotFound page. Because VC Frontend is served via a catch-all route, the document HTTP status is **200 by default (a documented "soft 404")** — a real HTTP 404/410 is produced only via load-balancer/CDN rules. The missing slug is logged as a broken link where an admin can assign a 301 redirect (surfaced as `slugInfo.redirectUrl`). The page must never show the deleted product's old content, blank/broken content, or a 500.
- **Verify:** Note the product URL → delete the product in Admin (or use a never-existing slug) → navigate to the URL → xAPI `slugInfo` entityInfo is empty and the SPA shows the NotFound page (HTTP 200 soft 404 by default, or a configured 301 redirect / CDN-forced 404). Never the old product content, a blank page, or a 500.
- **Violation signal:** Deleted product's old content still served; blank/broken content; 500 error; the product page still fully resolving after deletion. **NOTE:** HTTP 200 with the SPA NotFound page is the documented default and is **NOT** a violation.
- **Agents:** qa-frontend-expert (URL navigation), qa-backend-expert (routing/SEO config)
