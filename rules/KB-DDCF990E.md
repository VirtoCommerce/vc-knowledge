---
id: KB-DDCF990E
subject: BL-PRICE-009 `discountPercent` is a 4-decimal fraction, rounded away-from-zero, independent of the money rounding policy
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: PriceType.discountPercent
evidence:
  - method: observation
    at: 2026-09-17T15:21:00.497Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-PRICE-009: `discountPercent` is a 4-decimal fraction, rounded away-from-zero, independent of the money rounding policy `[P2-ux]`

- **Rule:** The `discountPercent` field (backing model `ProductPrice.DiscountPercent`, exposed via GraphQL `PriceType.discountPercent`) is computed as `discountAmount / listPrice`, rounded to exactly **4 decimal places** using **away-from-zero** midpoint rounding — hardcoded, and NOT routed through the pluggable money-rounding-policy extension point (that policy governs currency `MoneyType` amounts only, not this raw decimal fraction). When `listPrice` is zero, the value is `0`. The field is a **fraction** (e.g. `0.1250`), never a whole-number percentage, and is never `null`.
- **Verify:** Query a product's `price { discountPercent }` where a discount is active. Compute `round(discountAmount / listPrice, 4, AwayFromZero)` independently and assert equality. Confirm the value carries up to 4 decimal digits rather than being pre-multiplied by 100 or truncated to fewer decimals. A product with no discount returns `0`, not `null`.
- **Violation signal:** `discountPercent` returned as a whole number instead of a fraction; precision truncated below 4 decimals; a midpoint value rounded to-even instead of away-from-zero; `null` on a no-discount product; or the value changing after a custom money-rounding policy is registered (it must not — this field bypasses that policy entirely).
- **Agents:** qa-backend-expert
- **Docs:** N/A — implementation-detail; no VirtoOZ guide narrates this field's precision or rounding mode (§1a).
- **Source:** vc-module-x-api `src/VirtoCommerce.Xapi.Core/Models/ProductPrice.cs` — `private const int _discountPercentDecimalDigits = 4;` and `GetDiscountPercent() => ListPrice.Amount > 0 ? Math.Round(DiscountAmount.Amount / ListPrice.Amount, 4, MidpointRounding.AwayFromZero) : 0`; wired 1:1 in vc-module-x-catalog `src/VirtoCommerce.XCatalog.Core/Schemas/PriceType.cs` (`Field(d => d.DiscountPercent, nullable: false)`).
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; MISSING → new entry. Docs N/A per §1a; Source + Live agree. Note: the shipped implementation deliberately does NOT reuse `IMoneyRoundingPolicy` — a review comment established that cash-rounding intervals would corrupt a percentage ratio.)

---
