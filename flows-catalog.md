# Flows

Procedures written through `kb capture --flow` and served by `kb how`, never by `kb ask`. 3 active flows.

A flow is identified by its GOAL and its scope, not by the coordinates it touches: "place an
order" and "cancel an order" travel the same routes and are not the same procedure. It is
searched from its own index, because a procedure names the generic nouns of a whole journey
and would otherwise be a plausible answer to most questions asked in ordinary words.

| id | goal | confirmations | disputed | scope |
|---|---|---|---|---|

| id | goal | confirmations | disputed | scope |
|---|---|---|---|---|
| [`KB-A54C919F`](flows/KB-A54C919F.md) | `make a promotion require a coupon and use the code on the storefront` | 1 | no | surface=admin-ui surface=storefront-ui |
| [`KB-AFB2D3C5`](flows/KB-AFB2D3C5.md) | `an order placed on the storefront and read back in Admin` | 5 | no | surface=storefront-ui |
| [`KB-EB228603`](flows/KB-EB228603.md) | `create a percentage-off promotion and see it apply on the storefront` | 3 | no | surface=admin-ui |
