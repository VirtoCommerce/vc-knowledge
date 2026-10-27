---
id: KB-7568FFBB
subject: "On vcptcore-qa three recurring Hangfire jobs fail every day: AutoAccountLockoutJob and PruneExpiredTokensJob (abstract class instantiation) and AbandonedCartReminderJob.Process, accumulating 612 failed jobs"
plane: experiential
question: Which recurring Hangfire jobs fail daily on vcptcore and why is the Failed count in the hundreds?
status: active
appliesTo:
  - axis: account
    value: admin
  - axis: surface
    value: hangfire-dashboard
anchors:
  - coordinate: /hangfire/jobs/failed
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; module:VirtoCommerce.BackgroundJobs=3.1054.0
    defect: suspected
    at: 2026-10-27T20:37:09.173Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
    note: "distinct: Different stand and build, and a third failing recurring job (AutoAccountLockoutJob) the vcst_qa entry does not list."
---
2026-10-27, platform 3.1085.0, BackgroundJobs 3.1054.0. /hangfire (admin session) showed Failed 612, Succeeded 111,075, Servers 2, Enqueued/Scheduled/Processing 0. /hangfire/jobs/failed lists a repeating daily triple: 'Recurring: AutoAccountLockoutJob' fails with System.InvalidOperationException "Instances of abstract classes cannot be created." from ActivatorUtilities.CreateInstance; 'Recurring: PruneExpiredTokensJob' fails the same way about 4 hours earlier; AbandonedCartReminderJob.Process fails as well. Compared with vcst_qa (KB-4803E670), AutoAccountLockoutJob is an additional failing recurring job here. A smoke assertion of Failed < 5 cannot hold on this stand.
