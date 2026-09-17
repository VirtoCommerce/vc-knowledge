---
id: KB-CBD11DE1
subject: BL-WL-002 Org & store settings merge per-field, org-preferred (NOT whole-object override)
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:25.213Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-WL-002: Org & store settings merge per-field, org-preferred (NOT whole-object override) `[P1-data]`

- **Rule:** Org and store WL settings are merged **field by field**. For each of `logoUrl`, `secondaryLogoUrl`, `faviconUrl`, `themePresetName`, `footerLinkListName`, `mainMenuLinkListName`, the org value is used when non-empty, otherwise the store value. An org that sets only some fields inherits the store's remaining fields — it is not a whole-object override.
- **Verify:** Org sets logo only; store sets theme + footer → merged result = org logo + store theme + store footer. (Live-verified 2026-07-02: Electronics org @ B2B-store → org's Watermelon theme replaces store Coffee while other fields merge.)
- **Violation signal:** Setting one org field blanks out store-provided fields; or org fields ignored entirely.
- **Agents:** qa-backend-expert, qa-frontend-expert
- **Source:** `GetCombinedWhiteLabelingSetting()` — per-field ternaries `!IsNullOrEmpty(org.X) ? org.X : store.X` + `WhiteLabelingFlags` (HasLogo/HasSecondaryLogo/HasFavicon) picks. Refines the old BL-B2B-006 "override" wording.
- **Promoted:** 2026-07-02 (TLC-2026-07-02-2043).
