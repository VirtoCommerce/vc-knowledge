---
id: KB-650B8BFB
subject: Deleting or cancelling an order never releases a coupon use; deleting the promotion removes its usages
plane: experiential
question: Does cancelling or deleting an order give the coupon use back?
questions:
  - text: Is a coupon use released when the order that used it is cancelled?
  - text: How do I reset a coupon's usage count to zero?
  - text: Are PromotionUsage records deleted with the promotion?
status: active
appliesTo:
  - axis: module
    value: marketing
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/order/customerOrders
  - coordinate: DELETE /api/marketing/promotions
evidence:
  - method: source
    deployment: vc-module-marketing@0f68136
    at: 2026-11-03T16:29:39.967Z
    by: session:05e86aa3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    conditions: module:VirtoCommerce.SalesRep=3.1017.0-pr-30-1e10; module:VirtoCommerce.Marketing=3.1010.0; store=B2B-store
    at: 2026-11-03T16:53:09.133Z
    by: session:05e86aa3
    bodyHash: 539c840b44a6
    who: Lenajava1
    note: "Observed live 2026-11-03: in-case coupon cap 3, one order with a (promotionId, coupon) discount -> totalUsesCount 1 (admin coupons/search and salesRepCoupons). PUT the order with isCancelled=true, status Cancelled -> both counts stayed 1 for 124 s (9 samples); DELETE /api/order/customerOrders?ids= -> still 1 after 30 s. Deleting the promotion removed everything."
---
CouponUsageRecordHandler handles OrderChangedEvent entries with EntryState Added only and always calls RecordUsages with an empty oldUsages list, so its removal branch never runs: an order that is later cancelled, edited or deleted keeps its recorded usage, and IPromotionUsageService.DeleteAsync has no other caller in the module. The PromotionUsage table's FK to Promotion is ON DELETE CASCADE (MarketingDbContext), so deleting the promotion removes its usages; that is the only way to bring a coupon's count back to zero. Read in source; not observed live.
