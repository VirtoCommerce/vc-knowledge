---
id: KB-1FDE65F5
subject: BL-BOPIS-007 BOPIS store-selector map does not collapse on no-results search
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:15.268Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-BOPIS-007: BOPIS store-selector map does not collapse on no-results search `[P2-ux]`

- **Rule:** When the store-selector modal's search returns no results, the map panel must remain visible and must NOT collapse to zero width or a hidden state. Mechanism (vc-frontend source: `shared/checkout/components/select-address-map/select-address-map-desktop.vue`): the side-by-side layout renders whenever `(addresses.length || filterIsApplied)` is true, so a no-results **search** (filter applied, zero results) keeps both panels mounted — the list sidebar is a fixed width (Tailwind `w-60` / 240 px, `shrink-0`) and the map wrapper is `grow` with the map view inside it (`data-test-id="pickup-locations-map"`, `h-full`) filling the remaining modal width and never shrinking on a no-results query. The no-results message + Reset-search button render inside the fixed-width list panel (`data-test-id="pickup-locations-not-found"` / `"reset-search-button"`), not the map. The full map-replacing not-found placeholder appears only when there are genuinely NO locations AND no filter is applied — a different state. NOTE: the ≥40% / baseline ~50% figures are a conservative live-measured floor, not an enforced CSS token; with a fixed 240 px sidebar and a `grow` map, on desktop the map is in practice well over half the modal width.
- **Verify:** Open the BOPIS store-selector modal → measure the map panel (`data-test-id="pickup-locations-map"`) width via `browser_evaluate` + `getBoundingClientRect()` → search for a guaranteed no-match term → the no-results message (`data-test-id="pickup-locations-not-found"`) appears in the list panel → the map panel remains visible and does not shrink (≥40% of modal width as a conservative floor).
- **Violation signal:** Map panel collapses to < 40% of modal width after no-results search; map panel hidden entirely; map width measured at 0px after search; map panel width decreases on no-results but not on results.
- **Cross-reference:** VCST-4518 (map collapse regression)
- **Agents:** qa-frontend-expert (BOPIS modal layout), ui-ux-expert (layout measurement)
