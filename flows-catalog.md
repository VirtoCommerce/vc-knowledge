# Flows

Procedures written through `kb capture --flow` and served by `kb how`, never by `kb ask`. 2 active flows.

A flow is identified by its GOAL and its scope, not by the coordinates it touches: "place an
order" and "cancel an order" travel the same routes and are not the same procedure. It is
searched from its own index, because a procedure names the generic nouns of a whole journey
and would otherwise be a plausible answer to most questions asked in ordinary words.

| id | goal | confirmations | disputed | scope |
|---|---|---|---|---|
| [`KB-AFB2D3C5`](flows/KB-AFB2D3C5.md) | `an order placed on the storefront and read back in Admin` | 3 | no | surface=storefront-ui |
| [`KB-EB228603`](flows/KB-EB228603.md) | `create a percentage-off promotion and see it apply on the storefront` | 1 | no | surface=admin-ui |
