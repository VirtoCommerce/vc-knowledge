---
id: KB-DCE51A2F
subject: BL-CROSS-009 Eventual consistency is bounded
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:12.080Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CROSS-009: Eventual consistency is bounded `[P1-data]`

- **Rule:** Any change to an entity — an admin edit (product, price, inventory, category, settings) **or a write through any API surface** (REST, xAPI/GraphQL) — must be reflected everywhere that entity is read within 120 seconds (2 reindex cycles). This bound covers search index, cache layers, CDN, **and per-entity read caches**: a write on one surface must invalidate the cached read of the *same* entity on every other surface — caches must expire on the entity's change event, not only on a same-surface write. After 120 seconds, any discrepancy (Admin vs storefront, or one API surface vs another) is a bug.
- **Verify:** (a) Make an admin change → start timer → check storefront repeatedly → reflects within 120s; if using CDN, verify cache purge within the same window. (b) Cross-surface: write an entity via one API (e.g. a REST cart change) → read the *same* entity via another API (e.g. a GraphQL cart query) → the read reflects the write, because the change event invalidated the read cache.
- **Violation signal:** Storefront shows stale data after 120s; change requires manual cache purge; inconsistency between search results and product detail pages; a read on one API surface returns stale data after a write on another surface (read cache not invalidated on the change event).
- **Agents:** qa-testing-expert (timing scenario), qa-frontend-expert (storefront), qa-backend-expert (search index; cross-surface API read-cache consistency)
- **Amended:** 2026-07-23 (generalized to cross-API-surface read-cache invalidation on the entity change event. Verified live: a REST cart write is reflected by a subsequent xAPI cart read once the cart-changed event expires the aggregate read cache. 3-source: {OBSERVED} live GREEN + source (a cart-changed event handler that expires the per-cart cache token) + fix ticket VCST-5505.)
