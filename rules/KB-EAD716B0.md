---
id: KB-EAD716B0
subject: BL-SRCH-002 Zero-result query shows an intact empty state
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:12.889Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SRCH-002: Zero-result query shows an intact empty state `[P2-ux]`

- **Rule:** When a search query returns zero results, the search-results page (rendered by `category.vue` → `category-products.vue`) must render an intact empty state and never a blank grid, broken layout, or error. It must display (1) a clear no-results message via a `VcEmptyView` (variant `search`, icon `outline-stock`) using i18n key `pages.catalog.no_products_filtered_message` when a keyword/filters are active (else `pages.catalog.no_products_message`), with the searched term echoed in the page heading via i18n key `pages.search.header_empty`; and (2) a recovery action — a reset button (i18n key `pages.catalog.no_products_button`) that clears the keyword/filters (emits `resetFilterKeyword`). NOTE: vc-frontend does **NOT** implement spelling "Did you mean…" suggestions nor a popular-products/categories fallback — do not assert them.
- **Verify:** Search for a nonsense term → the `VcEmptyView` no-results message shows with the term echoed in the heading → page layout intact → the reset button is present and clears the keyword/filters. (Do not assert a "Did you mean…" suggestion — it does not exist.)
- **Violation signal:** Blank product grid; broken layout on zero results; no `VcEmptyView`/message on zero results; error/500 on uncommon search terms.
- **Agents:** qa-frontend-expert (search results page), ui-ux-expert (UX evaluation)
- **Source:** vc-frontend `vc-empty-view.vue` (`VcEmptyViewVariantType "search"`); RESET SEARCH clears keyword/filters. Live zero-result state confirmed intact.
- **Amended:** 2026-07-22 (triangulated — BL-AUDIT-2026-07-22; CONFIRMED 3/3, Source anchor recorded, Rule unchanged)
