---
id: KB-D9B149AD
subject: In Organization balance mode a loyalty mission's progress is shared by the whole organization, so a member's first order can find a mission already Completed by a colleague
plane: experiential
question: With Loyalty.LoyaltyBalanceCalculationMode = Organization, is loyalty mission progress per user or shared by the organization?
questions:
  - text: Why does a zero-history buyer see a loyalty mission already completed?
  - text: Who owns loyalty mission progress in organization points calculation mode?
  - text: Does a second org member's order add to a mission another member already completed?
concepts:
  - id: loyalty-mission
status: active
appliesTo:
  - axis: module
    value: loyalty
  - axis: store
    value: b2b-store
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/loyalty-mission-progress/search
  - coordinate: /account/missions
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 0a0b8dac8ab16bcc
    conditions: platform=3.1076.8; module:VirtoCommerce.Loyalty=3.1009.0; setting:Loyalty.LoyaltyBalanceCalculationMode=Organization
    at: 2026-11-09T00:20:22.360Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
vcptcore_regression, B2B-store, Loyalty.LoyaltyBalanceCalculationMode=Organization. POST /api/loyalty-mission-progress/search returned progress records whose ownerId and organizationId are the order's organization, created by the first member's order. Member A's order (total 174) created these records at 23:43 and completed every OrderValue mission with a target of 174 or less. Five minutes later, member B of the same org placed their first order (total 24). B's storefront missions then showed those missions at 100% / Completed, which is the shared org progress. The Completed records kept currentValue 174 (B's order added nothing to them), while the org's InProgress records went to 198. Two days earlier the same missions had user-owned records (organizationId null), so this env's mode was switched between runs. The docs agree: managing-loyalty-missions.md §Switch between individual and organization missions. Source: ProcessOrderAsync uses order.OrganizationId when the store is in organization mode. Consequence for tests: a per-user zero-history account is not isolated in this mode, and each case needs its own organization.
