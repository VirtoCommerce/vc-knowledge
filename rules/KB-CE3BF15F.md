---
id: KB-CE3BF15F
subject: BL-CAT-004 Category visibility toggle
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:09.380Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CAT-004: Category visibility toggle `[P2-ux]`

- **Rule:** Setting a category to "invisible" (visible=false) in Admin hides it from storefront navigation menus and category pages. However, products within a hidden category remain accessible via direct URL, search, and other categories they belong to. Subcategories of a hidden category also become hidden from navigation.
- **Verify:** Hide category in Admin → storefront menu no longer shows it → products still accessible via search or direct URL → subcategories also hidden from nav. Unhide → category and subcategories return to nav.
- **Violation signal:** Hidden category still in navigation menu; products in hidden category inaccessible via direct URL; subcategories still visible when parent is hidden.
- **Agents:** qa-frontend-expert (storefront nav), qa-backend-expert (category API)
