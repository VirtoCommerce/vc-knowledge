---
id: KB-A94ADBF9
subject: BL-PROFILE-001 Silent duplicate-skip on `updateMemberAddresses` and matching `checkDuplicateAddress` detection
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: Mutations.updateMemberAddresses
evidence:
  - method: observation
    at: 2026-09-17T15:21:18.277Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-PROFILE-001: Silent duplicate-skip on `updateMemberAddresses` and matching `checkDuplicateAddress` detection `[P1-data]`

- **Rule (write path — `updateMemberAddresses`):** When `updateMemberAddresses` is called with an address whose key fields — `firstName` + `lastName` + `city` + `line1` + `line2` + `countryCode` + `regionId` + `postalCode` + `phone` + `email` (compared case-insensitively; `addressType` and the auto-computed `name` are **NOT** part of the dedup key) — exactly match an already-saved address on the same member, the server MUST silently skip the insert. No new record is created, no error is raised in `errors[]`, and the member's total address count (`currentCustomerAddresses.totalCount`) MUST remain unchanged. This holds regardless of the size of `addresses[]` (one or many) AND regardless of `memberType` (Contact or Organization — same endpoint, same dedup semantics). The dedup check is **against the member's stored collection**, not only within the incoming batch, and must NOT depend on auto-computed fields like `name` that the client submits as null.
- **Rule (read path — `checkDuplicateAddress`):** `checkDuplicateAddress(memberId, address)` MUST return `isDuplicated: true` if and only if an existing stored address on `memberId` matches the submitted address by the same key fields listed above. Novel addresses return `isDuplicated: false`; exact matches return `true`. The detection contract MUST agree with the write-path dedup contract — whatever `updateMemberAddresses` silently skips, `checkDuplicateAddress` must flag. The query MUST require authentication (no anonymous access) and MUST enforce same-member / same-org authorization (no cross-member probing).
- **Verify (write path):** Capture totalCount = N and the full field set of an existing address. Call `updateMemberAddresses(command: { memberId, addresses: [{…same fields}] })` with exactly one byte-identical element. Re-query totalCount → must equal N. Count rows in `items[]` matching the duplicate's line1 + firstName + lastName → must equal 1 (not 2). `errors[]` must be empty. Repeat with a 2-element `addresses[]` where one element is identical-to-existing and one is novel → novel row is added, duplicate is skipped, totalCount = N+1. Repeat both scenarios for a Contact memberId AND an Organization memberId.
- **Verify (read path):** With a valid bearer token, call `checkDuplicateAddress(memberId: <own>, address: {…byte-identical fields of an existing saved address})` → `isDuplicated: true`. Call with a novel address → `isDuplicated: false`. Call anonymously (no Authorization header) → request rejected with 401 or equivalent authz error; not HTTP 200. Call with a foreign memberId (different user) → authz error, no data returned.
- **Violation signal:** `totalCount` = N+1 after single-element submission; two rows with identical key fields appear in `items[]`; the mutation raises an error instead of silently skipping. For the read path: `checkDuplicateAddress` returns `isDuplicated: false` for an address that clearly exists on the member; or returns data to an unauthenticated caller (HTTP 200 without 401); or returns data when a foreign memberId is used.
- **Agents:** qa-backend-expert (GraphQL direct — see GQL-056, and planned GQL-060/061 for checkDuplicate detection), qa-frontend-expert (storefront UI — see B2C-SHIP-014), test-management-specialist (cross-layer coverage audit)
- **Origin:** PR [VirtoCommerce/vc-module-profile-experience-api#129](https://github.com/VirtoCommerce/vc-module-profile-experience-api/pull/129) — adds both `MemberAggregateRootBase.UpdateAddresses` dedup AND the `checkDuplicateAddress` query, implemented once in the shared base aggregate (no per-member-type override), so the Contact and Organization paths use identical logic and the write-path silent-skip and read-path detection share one method (`IsDuplicateAddress`). The previously-reported Organization-path write-dedup miss and `checkDuplicateAddress`-always-false defects are **no longer reproducible in current source** (both resolved via the unified base aggregate); live-reconfirmed on the Contact path.
- **Promoted:** 2026-04-23 (from `PROPOSED-BL-PROFILE-001` in `reports/test-lifecycle/TLC-2026-04-23-1700/bl-proposals.md`).
- **Source:** `vc-module-profile-experience-api` `MemberAggregateRootBase.cs` (address comparer + `IsDuplicateAddress`), `CheckDuplicateAddressQueryHandler.cs` (delegates to the same method), `OrganizationAggregate.cs` / `ContactAggregate.cs` (no override — inherit the shared logic).
- **Amended:** 2026-07-22 (approved from bl-proposals-2026-07-22 — BL-AUDIT-2026-07-22: refreshed the stale Origin footnote — the Organization-path + `checkDuplicateAddress` defects are fixed in current source, triangulated source + live Contact path; Rule/Verify unchanged).

---
