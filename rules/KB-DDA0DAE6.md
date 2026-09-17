---
id: KB-DDA0DAE6
subject: BL-CAT-012 Category dictionary-value & metadata management
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:10.634Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CAT-012: Category dictionary-value & metadata management `[P2-ux]`

- **Rule:** Adding or removing a category **tax-type dictionary value**, **SEO** record (store-scoped), **image**, or **localized description** persists to the category and is **scoped to the value acted on** — deleting one dictionary value must not remove other shared values. SEO/description changes render on the storefront, respecting locale.
- **Verify:** Add a tax-type value → in dropdown + `GET …/{id}`. Delete a self-created value → only that value gone, shared values intact. Add SEO/image/description → persists + renders on the storefront for the correct locale.
- **Violation signal:** Save fails silently; a shared/pre-existing dictionary value deleted instead of the target; SEO/description not rendered on storefront; localized description shown under the wrong locale.
- **Agents:** qa-backend-expert (Catalog API, Admin SPA), qa-frontend-expert (storefront SEO/description render)

---
