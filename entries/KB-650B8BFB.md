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
---
CouponUsageRecordHandler handles OrderChangedEvent entries with EntryState Added only and always calls RecordUsages with an empty oldUsages list, so its removal branch never runs: an order that is later cancelled, edited or deleted keeps its recorded usage, and IPromotionUsageService.DeleteAsync has no other caller in the module. The PromotionUsage table's FK to Promotion is ON DELETE CASCADE (MarketingDbContext), so deleting the promotion removes its usages; that is the only way to bring a coupon's count back to zero. Read in source; not observed live.
