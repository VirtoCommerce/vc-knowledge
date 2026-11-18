---
id: KB-32FABE4B
subject: "ProductSubscription.ReminderLeadDays store override is honoured by POST scheduler/run: with 1, a subscription due in 2 days gets no renewal; due tomorrow gets a renewal, ReminderSent and a RenewalReminderEmailNotification"
plane: experiential
question: Does POST api/product-subscription/scheduler/run use the store's ProductSubscription.ReminderLeadDays override when deciding which subscriptions get a renewal and reminder?
questions:
  - text: How many days before the next order date is a product subscription renewal created and the reminder email sent?
  - text: Can a store override ProductSubscription.ReminderLeadDays and how do you restore it?
  - text: What next order date does PUT api/product-subscription/subscriptions/{id} accept?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/product-subscription/scheduler/run
  - coordinate: PUT /api/product-subscription/subscriptions/{id}
  - coordinate: PUT /api/stores
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 9dfbb57eacb0bf2c
    conditions: module=VirtoCommerce.ProductSubscription@3.1000.0-pr-1-79cf; cutoffHours=24; storeReminderLeadDays=1
    at: 2026-11-17T11:00:11.658Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: ad6ae9902bd60819
    conditions: setting:ProductSubscription.ReminderLeadDays=1 (store B2B-store); setting:ProductSubscription.CutoffHours=24 (default)
    at: 2026-11-18T15:15:19.746Z
    by: session:d9b8e380
    bodyHash: 73dcf0d13245
    who: kutasinaelena
    note: "2026-11-18: PUT nextOrderDate today -> 400 'between tomorrow and one year'; +2d -> processed 0, no renewal; +~23h -> renewal with ReminderSent event + RenewalReminderEmailNotification, then Authorized in the same run (inside cutoff)."
---
As admin via REST on store B2B-store (global ReminderLeadDays unset, default 3). Store override set to 1 through a full-body PUT /api/stores (POST /api/platform/settings with objectType Store persisted the value — the v2 tenant values read 1 — but GET /api/stores kept returning null, i.e. stale store cache, so the scheduler could read the old value). A USER2 subscription's next order date moved via PUT api/product-subscription/subscriptions/{id} to now+2d, then POST scheduler/run: processed 0, no renewal. Next date then moved to tomorrow 01:00 UTC (PUT refuses a date earlier than tomorrow's UTC calendar date: 400 "The next delivery date must be between tomorrow and one year from now."), scheduler/run: processed 1, renewal created with events StateChanged/Calculated/ReminderSent and RenewalReminderEmailNotification in the notification journal (rendered subject "Your next delivery: Nov 18"); because the delivery was inside CutoffHours=24, the same run also placed and authorized the renewal order. Restoring the inherited state is not possible via REST: writing null (store PUT or v2 tenant values POST) leaves the override, an empty string makes PUT /api/stores return 500, and there is no delete endpoint — the override can only be set back to the default value explicitly.
