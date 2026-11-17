---
id: KB-CF22D2D4
subject: The product-subscription annual reminder is due one year after a subscription's LastAnnualReminderDate (or StartDate), independent of interval and ReminderLeadDays, and no REST call can backdate either date
plane: experiential
question: When does POST api/product-subscription/scheduler/run send SubscriptionAnnualReminderEmailNotification, and can a test make it due?
questions:
  - text: Does ProductSubscription.AnnualReminder.Enabled send a reminder for yearly-interval subscriptions before renewal?
  - text: How to test the product subscription yearly reminder email
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/product-subscription/scheduler/run
  - coordinate: PUT /api/product-subscription/subscriptions/{id}
evidence:
  - method: source
    deployment: VirtoCommerce.ProductSubscription@3.1000.0-pr-1-79cf
    at: 2026-11-17T11:00:27.071Z
    by: session:d9b8e380
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 959440e006132a35
    conditions: platform=3.1099.0; module:VirtoCommerce.ProductSubscription=3.1000.0-pr-1-79cf; store=B2B-store
    at: 2026-11-17T12:32:47.307Z
    by: session:d9b8e380
    bodyHash: f136b54acb23
    who: kutasinaelena
    note: "Observed live after a DB backdate of StartDate/LastAnnualReminderDate by 13 months on two Active and one Cancelled subscription of one customer: store AnnualReminder.Enabled=false → scheduler/run sent nothing, dates unchanged; =true → one SubscriptionAnnualReminderEmailNotification (\"Your subscriptions at B2B-store: yearly reminder\") listing both Active subscriptions (yearly and monthly), Cancelled excluded, LastAnnualReminderDate set to now on both; next run sent no duplicate. scheduler/run reported processed 0 (count excludes reminders)."
---
RenewalProcessor.SendAnnualRemindersAsync runs at the end of every scheduler run for each store with ProductSubscription.AnnualReminder.Enabled (store setting, default true): it selects Active/Paused subscriptions with (LastAnnualReminderDate ?? StartDate) <= now-1y, sends one SubscriptionAnnualReminderEmailNotification per customer group and sets LastAnnualReminderDate=now. It is not tied to the interval unit (Year) nor to ReminderLeadDays. Checkout sets LastAnnualReminderDate=StartDate=now (observed live: lastAnnualReminderDate equals startDate on a new subscription); legacy import sets LastAnnualReminderDate=now; PUT subscriptions/{id} binds only SubscriptionChange (no date fields besides nextOrderDate). So the email cannot be made due through the API — only by a DB backdate or waiting a year.
