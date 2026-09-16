# Captured

Written by agents through `kb capture`, not generated. 82 active entries, 12 retired.

The confirmation count, the disputed flag and the versions each fact has been seen on are read
out of `evidence[]`. Nothing here declares them.

SECTIONS EXIST SO THE LIST STAYS READ. This catalog is meant to be handed to an agent whole,
and what fails as it grows is the reading, not the context window. Sections are derived from
subject, question and anchors (`src/topics.mjs`), so a wrong filing is visible here rather than
hidden in a table. An entry is filed under one section and its other topics are named beside
it: 27 of 78 touch more than one, so these are tags, not folders.

Within a section: disputed first, then by independent confirmations. A reader who stops early
should stop on what most parties have seen, and on what somebody disagrees with.

## members & accounts — 24

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-4D082C89`](captured/KB-4D082C89.md) | `a pending invitation is a locked account with no status` | 2 | yes (3) | surface=storefront-ui surface=admin-ui | — |
| [`KB-FA724D31`](captured/KB-FA724D31.md) | `storefront Delete member detaches the contact and orphans the account` | 2 | yes (1) | surface=storefront-ui surface=admin-ui | — |
| [`KB-27B4CD10`](captured/KB-27B4CD10.md) | `storefront members Active column reads contact status not account state` | 4 | no | surface=storefront-ui surface=storefront-xapi | — |
| [`KB-4B889114`](captured/KB-4B889114.md) | `company members Active column vs account locked state` | 4 | no | surface=storefront-ui surface=admin-ui principal=org-maintainer | — |
| [`KB-06409954`](captured/KB-06409954.md) | `three independent fields represent a blocked organization member` | 2 | no | surface=storefront-ui surface=admin-ui | — |
| [`KB-7040852E`](captured/KB-7040852E.md) | `member keyword search is index-backed` | 2 | no | surface=admin-ui | — |
| [`KB-B769C7B1`](captured/KB-B769C7B1.md) | `storefront org role and platform user role are one store` | 2 | no | surface=storefront-ui surface=admin-ui | stores & tax |
| [`KB-02238DE5`](captured/KB-02238DE5.md) | `storefront org-role gating granularity` | 1 | no | surface=storefront-ui principal=org-member | — |
| [`KB-0B6067F8`](captured/KB-0B6067F8.md) | `UserType.lockedState is the storefront-reachable sign-in state` | 1 | no | surface=storefront-xapi | — |
| [`KB-132A40B3`](captured/KB-132A40B3.md) | `a null lastLoginDate cannot tell never-tried from tried-and-failed` | 1 | no | surface=admin-ui | — |
| [`KB-4A8606CA`](captured/KB-4A8606CA.md) | `an abandoned invitation can leave an account nothing can delete` | 1 | no | surface=rest surface=admin-ui surface=storefront-ui | — |
| [`KB-4E45A8BA`](captured/KB-4E45A8BA.md) | `admin contact Status picker cannot represent Invited or Locked` | 1 | no | surface=admin-ui | — |
| [`KB-6CC1EFE6`](captured/KB-6CC1EFE6.md) | `deleting a security account leaves the contact behind` | 1 | no | surface=admin-ui | — |
| [`KB-82111688`](captured/KB-82111688.md) | `the storefront members Status filter finds nobody in a state the roster is full of` | 1 | no | surface=storefront-ui | — |
| [`KB-ADFD93AB`](captured/KB-ADFD93AB.md) | `org-specific pricing runs through the organization's user groups, not through organizationId` | 1 | no | surface=admin-ui surface=storefront-ui principal=org-member | — |
| [`KB-BAEBCDA7`](captured/KB-BAEBCDA7.md) | `an organization invitation cannot be cancelled or resent` | 1 | no | surface=storefront-ui surface=admin-ui principal=org-maintainer | — |
| [`KB-BC6FC633`](captured/KB-BC6FC633.md) | `admin account role assignment is staged until save` | 1 | no | surface=admin-ui | — |
| [`KB-BCA7468D`](captured/KB-BCA7468D.md) | `two endpoints disagree about whether a password hash is a secret` | 1 | no | surface=rest | — |
| [`KB-D24EDC70`](captured/KB-D24EDC70.md) | `what refuses a never-registered account at sign-in was not established, and here is where the looking stopped` | 1 | no | surface=storefront-ui | — |
| [`KB-DA14E8B7`](captured/KB-DA14E8B7.md) | `block on an un-accepted invitation is irreversible` | 1 | no | surface=storefront-ui surface=storefront-xapi principal=org-maintainer | — |
| [`KB-E17E4CEF`](captured/KB-E17E4CEF.md) | `admin account Status picker cannot represent the values it displays` | 1 | no | surface=admin-ui | — |
| [`KB-EAA7BA2F`](captured/KB-EAA7BA2F.md) | `storefront Lists page is an owner-only roster` | 1 | no | surface=storefront-ui | — |
| [`KB-EF3CB7FB`](captured/KB-EF3CB7FB.md) | `Login on behalf is authorized on the storefront not in Admin` | 1 | no | surface=admin-ui surface=storefront-ui | — |
| [`KB-BF730613`](captured/KB-BF730613.md) | `lockoutEnd has three meaningful values, and one of them looks like the opposite of what it means` | 0 | no | surface=rest | — |

## promotions & discounts — 19

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-35A09C64`](captured/KB-35A09C64.md) | `promotion re-evaluation on cart read` | 5 | no | surface=storefront-xapi | cart & checkout |
| [`KB-996BDF08`](captured/KB-996BDF08.md) | `cart-level promotion reward placement` | 4 | no | surface=storefront-xapi | cart & checkout, orders & shipments |
| [`KB-4982C91F`](captured/KB-4982C91F.md) | `order-discount-amount-rounding-split` | 3 | no | surface=rest surface=admin-ui reward=percentage-off-cart-subtotal | orders & shipments |
| [`KB-5ADBFB34`](captured/KB-5ADBFB34.md) | `only the largest cart-subtotal promotion applies, whatever isExclusive says` | 3 | no | surface=rest | cart & checkout |
| [`KB-D4A064A5`](captured/KB-D4A064A5.md) | `promotion discount rounding on the cart` | 3 | no | surface=storefront-xapi | cart & checkout |
| [`KB-CA4C93E4`](captured/KB-CA4C93E4.md) | `discount-rate-not-persisted-on-order` | 2 | no | surface=storefront-xapi surface=rest principal=customer | orders & shipments |
| [`KB-E7790BF6`](captured/KB-E7790BF6.md) | `where an applied coupon and its reward live on a cart` | 2 | no | surface=storefront-xapi surface=rest principal=customer | cart & checkout |
| [`KB-F1542157`](captured/KB-F1542157.md) | `an order-level discount is not allocated to the line items` | 2 | no | surface=admin-ui | orders & shipments |
| [`KB-FF7E4D5B`](captured/KB-FF7E4D5B.md) | `cart-subtotal-percentage-reward-on-customerorder` | 2 | no | surface=storefront-xapi surface=rest principal=customer reward=percentage-off-cart-subtotal | cart & checkout, orders & shipments |
| [`KB-35F20D97`](captured/KB-35F20D97.md) | `what a coupon code that does not work does to a cart` | 1 | no | surface=storefront-xapi | cart & checkout |
| [`KB-5790A068`](captured/KB-5790A068.md) | `marketing promotions are not change-tracked on this deployment` | 1 | no | surface=rest | — |
| [`KB-8264632C`](captured/KB-8264632C.md) | `how the storefront cart page renders an applied and an unapplied coupon` | 1 | no | surface=storefront-ui principal=customer | cart & checkout |
| [`KB-A3CE7FBA`](captured/KB-A3CE7FBA.md) | `recovering a percentage rate from a discount amount` | 1 | no | surface=storefront-xapi reward=percentage-off-cart-subtotal | cart & checkout |
| [`KB-AD1FA66B`](captured/KB-AD1FA66B.md) | `where a cart's money lives, and which copy goes stale` | 1 | no | surface=rest surface=storefront-xapi principal=customer | cart & checkout |
| [`KB-C1024558`](captured/KB-C1024558.md) | `telling a coupon-driven discount from an automatic one on a cart` | 1 | no | surface=storefront-xapi reward=percentage-off-cart-subtotal | cart & checkout |
| [`KB-D60012DB`](captured/KB-D60012DB.md) | `discount-row-withtax-is-never-written` | 1 | no | surface=rest | orders & shipments |
| [`KB-D992AF44`](captured/KB-D992AF44.md) | `order discount row is a snapshot, not a live reference` | 1 | no | surface=rest surface=storefront-xapi surface=admin-ui | orders & shipments |
| [`KB-E01A9F11`](captured/KB-E01A9F11.md) | `the promotion blade's required-field warnings are not a save gate` | 1 | no | surface=admin-ui surface=rest | — |
| [`KB-F027283D`](captured/KB-F027283D.md) | `the discount label a shopper sees is free text nobody keeps honest` | 1 | no | surface=storefront-ui surface=storefront-xapi surface=admin-ui | — |

## orders & shipments — 14

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-0C102D97`](captured/KB-0C102D97.md) | `cancelling an order cascades to the payment and never to the shipment` | 3 | no | surface=rest | — |
| [`KB-4CCC2DD6`](captured/KB-4CCC2DD6.md) | `what Cancel document on an order actually cancels` | 3 | no | surface=admin-ui | — |
| [`KB-0DD47BD1`](captured/KB-0DD47BD1.md) | `the amount a payment is for is not the payment's total` | 2 | no | surface=rest surface=admin-ui | — |
| [`KB-358A70CB`](captured/KB-358A70CB.md) | `storefront order page projection of the shipment` | 2 | no | surface=storefront-ui | — |
| [`KB-6AA0D7FB`](captured/KB-6AA0D7FB.md) | `fixed rate shipping method option pricing` | 2 | no | surface=storefront-ui surface=rest | — |
| [`KB-6E98AA17`](captured/KB-6E98AA17.md) | `admin order operations tree staleness after cancel` | 2 | no | surface=admin-ui | — |
| [`KB-16AEF0C2`](captured/KB-16AEF0C2.md) | `order operation numbering and parentage` | 1 | no | surface=admin-ui surface=rest | — |
| [`KB-18ABE89B`](captured/KB-18ABE89B.md) | `admin order address blade false dirty state` | 1 | no | surface=admin-ui | — |
| [`KB-4C5627CE`](captured/KB-4C5627CE.md) | `shipment lifecycle states and what moves them` | 1 | no | surface=admin-ui | — |
| [`KB-4D26A022`](captured/KB-4D26A022.md) | `order shipment item allocation` | 1 | no | surface=admin-ui surface=rest | — |
| [`KB-53D776C2`](captured/KB-53D776C2.md) | `the Admin order tree blanks a zero shipment amount` | 1 | no | surface=admin-ui | — |
| [`KB-7E35E6BC`](captured/KB-7E35E6BC.md) | `Admin renders order timestamps in local time while the API returns UTC` | 1 | no | surface=admin-ui | — |
| [`KB-CC9A98D3`](captured/KB-CC9A98D3.md) | `order address copies per operation` | 1 | no | surface=admin-ui surface=rest | — |
| [`KB-EC76F588`](captured/KB-EC76F588.md) | `there is no paymentMethodCode on an order payment, and shipment spells its sibling differently` | 0 | no | surface=rest | — |

## catalog & products — 13

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-3113CBC1`](captured/KB-3113CBC1.md) | `where a product's configurability is actually stored` | 2 | yes (1) | surface=rest | — |
| [`KB-360127D0`](captured/KB-360127D0.md) | `a configuration loses its product and quantity on the way to the order` | 1 | yes (1) | surface=graphql | orders & shipments |
| [`KB-C440D4E3`](captured/KB-C440D4E3.md) | `there is no product search route under /api/catalog/products` | 2 | no | surface=rest | — |
| [`KB-055845A3`](captured/KB-055845A3.md) | `a cart line can serve a unit price that is in neither the configuration nor the line's history` | 1 | no | surface=storefront-ui surface=storefront-xapi principal=customer | cart & checkout |
| [`KB-0C163966`](captured/KB-0C163966.md) | `the storefront product page of a configurable product` | 1 | no | surface=storefront-ui principal=customer | — |
| [`KB-1834ABE5`](captured/KB-1834ABE5.md) | `no Admin surface shows the price a given shopper would actually be charged` | 1 | no | surface=admin-ui principal=admin | — |
| [`KB-191B1B4C`](captured/KB-191B1B4C.md) | `the master product is a row in its own variations list` | 1 | no | surface=storefront-ui | cart & checkout |
| [`KB-59E4B5FC`](captured/KB-59E4B5FC.md) | `a configured product as an order line item` | 1 | no | surface=rest surface=graphql | orders & shipments |
| [`KB-7AB0B13E`](captured/KB-7AB0B13E.md) | `removing a product configuration` | 1 | no | surface=rest surface=admin-spa | — |
| [`KB-8A82CF4D`](captured/KB-8A82CF4D.md) | `switching a product's configurability on and off` | 1 | no | surface=rest surface=graphql | — |
| [`KB-A1B35892`](captured/KB-A1B35892.md) | `a configured product as a cart line item` | 1 | no | surface=graphql principal=customer | cart & checkout, orders & shipments |
| [`KB-CC195687`](captured/KB-CC195687.md) | `a variation on an order does not record its parent` | 1 | no | surface=rest surface=graphql | orders & shipments |
| [`KB-EB991080`](captured/KB-EB991080.md) | `configuration section options are filtered by storefront resolvability` | 1 | no | surface=graphql principal=customer | — |

## lists & sharing — 5

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-032FBB96`](captured/KB-032FBB96.md) | `SharingSettingType access is computed per caller` | 1 | no | surface=storefront-xapi | — |
| [`KB-3F7C78F6`](captured/KB-3F7C78F6.md) | `wishlists have no REST and no Admin surface` | 1 | no | surface=admin-ui surface=rest | — |
| [`KB-7A12CF52`](captured/KB-7A12CF52.md) | `what a wishlist sharing key grants a signed-out holder` | 1 | no | surface=storefront-ui principal=anonymous | members & accounts |
| [`KB-7A9D4927`](captured/KB-7A9D4927.md) | `revoking a wishlist share link: re-scoping suspends, deleting revokes` | 1 | no | surface=storefront-ui surface=storefront-xapi | — |
| [`KB-E367DA11`](captured/KB-E367DA11.md) | `wishlist scope has three values and the roster shows two` | 1 | no | surface=storefront-ui surface=storefront-xapi | — |

## cart & checkout — 3

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-A4EB3766`](captured/KB-A4EB3766.md) | `the cart record survives checkout, emptied and reused` | 1 | no | surface=rest principal=admin | orders & shipments |
| [`KB-AB35BCDC`](captured/KB-AB35BCDC.md) | `what an order line item stops carrying once the cart becomes an order` | 1 | no | surface=rest surface=graphql | orders & shipments |
| [`KB-EF925925`](captured/KB-EF925925.md) | `an open cart page is not re-confirmed before Place order` | 1 | no | surface=storefront-ui principal=customer | orders & shipments |

## stores & tax — 3

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-5F7C8FC4`](captured/KB-5F7C8FC4.md) | `an empty Tax providers widget is not evidence that a store has no tax provider` | 3 | no | surface=admin-ui | — |
| [`KB-6D5E2CD1`](captured/KB-6D5E2CD1.md) | `what makes a price list apply to a store, and what the storefront reads it through` | 1 | no | surface=admin-ui surface=rest surface=storefront-ui | — |
| [`KB-1B18B821`](captured/KB-1B18B821.md) | `tax is provider-driven and silently zero without an active provider` | 0 | no | surface=rest | orders & shipments |

## unfiled — 1

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-6824BC2B`](captured/KB-6824BC2B.md) | `platform GraphiQL runs as anonymous and takes a query in the URL` | 1 | no | surface=admin-ui principal=anonymous | — |

## retired — 12

Superseded or withdrawn. Kept so a reader who meets an id somewhere can find out what
happened to it; not part of the register an agent scans.

| id | subject | confirmations | disputed | scope | also |
|---|---|---|---|---|---|
| [`KB-0B1AD487`](captured/KB-0B1AD487.md) | `the base a cart-level percentage reward is taken on` _(retired)_ | 1 | no | surface=storefront-xapi reward=percentage-off-cart-subtotal | — |
| [`KB-1B230A62`](captured/KB-1B230A62.md) | `cart money is computed on read and persisted only on mutation` _(retired)_ | 1 | no | surface=rest surface=storefront-xapi principal=customer | — |
| [`KB-28579C5B`](captured/KB-28579C5B.md) | `a cart line's unit price is not re-evaluated on read, only on mutation` _(retired)_ | 1 | no | surface=storefront-ui surface=storefront-xapi principal=customer | — |
| [`KB-2C3CA126`](captured/KB-2C3CA126.md) | `what a configurable product's page offers a buyer` _(retired)_ | 1 | no | surface=storefront-ui principal=customer | — |
| [`KB-3691AC8D`](captured/KB-3691AC8D.md) | `order-discount-amount-precision` _(retired)_ | 1 | no | surface=rest surface=storefront-xapi reward=percentage-off-cart-subtotal | — |
| [`KB-469AA660`](captured/KB-469AA660.md) | `what cancelling an order does to its shipment and its payment` _(retired)_ | 1 | no | surface=rest surface=admin-spa | — |
| [`KB-48CF7684`](captured/KB-48CF7684.md) | `wishlist sharing key is minted at creation and never rotates` _(retired)_ | 1 | no | surface=storefront-ui surface=storefront-xapi | — |
| [`KB-80BDCBE9`](captured/KB-80BDCBE9.md) | `shipment lifecycle states and their controls` _(retired)_ | 1 | no | surface=admin-ui | — |
| [`KB-A646D086`](captured/KB-A646D086.md) | `discount-row-withtax-zero-without-tax-provider` _(retired)_ | 1 | no | surface=rest taxProvider=none | — |
| [`KB-BEA58773`](captured/KB-BEA58773.md) | `what the storefront cart page can and cannot say about a coupon` _(retired)_ | 1 | no | surface=storefront-ui principal=customer | — |
| [`KB-C51ACC81`](captured/KB-C51ACC81.md) | `placing an order on the storefront` _(retired)_ | 1 | no | surface=storefront-ui | — |
| [`KB-DC65E5F8`](captured/KB-DC65E5F8.md) | `blocking a pending invitation destroys the invitation` _(retired)_ | 1 | no | surface=storefront-ui surface=storefront-xapi principal=org-maintainer | — |
