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
---
RenewalProcessor.SendAnnualRemindersAsync runs at the end of every scheduler run for each store with ProductSubscription.AnnualReminder.Enabled (store setting, default true): it selects Active/Paused subscriptions with (LastAnnualReminderDate ?? StartDate) <= now-1y, sends one SubscriptionAnnualReminderEmailNotification per customer group and sets LastAnnualReminderDate=now. It is not tied to the interval unit (Year) nor to ReminderLeadDays. Checkout sets LastAnnualReminderDate=StartDate=now (observed live: lastAnnualReminderDate equals startDate on a new subscription); legacy import sets LastAnnualReminderDate=now; PUT subscriptions/{id} binds only SubscriptionChange (no date fields besides nextOrderDate). So the email cannot be made due through the API — only by a DB backdate or waiting a year.
