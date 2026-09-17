---
id: KB-C137EBBF
subject: BL-SEO-004 SEO link type controls URL format
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:18.099Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SEO-004: SEO link type controls URL format `[P1-data]`

- **Rule:** The store setting `seoLinkType` controls how product and category URLs are generated. When changed, all storefront URLs must update to the new format. Old-format URLs should either redirect (301) to the new format or return 404 — never serve content at both old and new URLs simultaneously (duplicate content penalty).
- **Verify:** Set `seoLinkType` to format A → note URLs. Change to format B → URLs update to new format. Navigate to old-format URL → 301 redirect to new format (or 404).
- **Violation signal:** Old and new format URLs both serve content (duplicate); URL format doesn't change after setting update; old URLs return 200 instead of 301/404; broken links after format change.
- **Agents:** qa-backend-expert (store settings API), qa-frontend-expert (URL navigation)

---
