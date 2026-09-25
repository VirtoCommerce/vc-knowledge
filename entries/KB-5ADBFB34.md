---
id: KB-5ADBFB34
subject: only the largest cart-subtotal promotion applies, whatever isExclusive says
plane: experiential
question: why did only one promotion apply when two were active, in scope and neither is exclusive
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: Marketing.Promotion.CombinePolicy
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-15T13:44:54.119Z
  - method: source
    module: VirtoCommerce.Marketing
    version: 3.1000.1
    path: src/VirtoCommerce.MarketingModule.Data/Services/EvaluationPolicies/BestRewardPromotionPolicy.cs
    url: https://raw.githubusercontent.com/VirtoCommerce/vc-module-marketing/3.1000.1/src/VirtoCommerce.MarketingModule.Data/Services/EvaluationPolicies/BestRewardPromotionPolicy.cs
    at: 2026-09-16T08:27:16.344Z
    by: claude-opus-5
  - method: observation
    deployment: vcptcore_stable
    platformVersion: 3.1007.27
    at: 2026-09-16T12:28:48+04:00
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/round2/arm-B/report.md
  - method: observation
    deployment: vcptcore_stable
    platformVersion: 3.1007.27
    at: 2026-09-16T12:28:48+04:00
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/round2/arm-C/report.md
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:34:10.651Z
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/round5/arm-A/REPORT.md
    note: "Round five's control arm, with no register: read Settings > Marketing > General and found 'Selected policy of using multiple active promotions' = BestReward; created two cart-subtotal promotions and saw exactly one discount line; raising the second from 5pct to 30pct flipped the winner and dropped the 20pct entirely, with the loser left Active and no trace."
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:15:40.098Z
    by: session:memimpor
    who: Lenajava1
    note: "Also seen on vcst-qa: under BestReward a small coupon-backed cart-subtotal reward replaced a much larger automatic one. Under CombineStackable only one CartSubtotalReward applies per priority group, so stacking two needs different priorities."
---

The store setting Marketing.Promotion.CombinePolicy decides, and its value here - also the platform default - is BestReward. MarketingModule selects CombineStackablePromotionPolicy only when that reads CombineStackable; otherwise BestRewardPromotionPolicy, which keeps at most ONE CartSubtotalReward for the whole cart: it orders the valid ones by GetTotalAmount and takes the first coupon-bearing one, else the first. Ordering is by MONEY, not by the raw Amount field, because AmountBasedReward.GetTotalAmount multiplies a Relative amount by the price. Observed: on a subtotal of 1612.97 a 15 percent relative reward is worth 241.9455 and a flat 50 dollar absolute reward is worth 50.00, so the flat one is discarded and never reaches result.Rewards - the order carries exactly one discount row, the 15 percent. isExclusive is a red herring in both directions: neither promotion was exclusive, so nothing was suppressed; the loser simply lost on size. Priority plays no part in this branch at all. Reproduced independently by three separate runs on 2026-09-15. To make both apply, the policy must be CombineStackable. Confirmed on platform 3.1007.27, Marketing 3.1000.1.
