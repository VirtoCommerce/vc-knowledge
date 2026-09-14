# Flows

Procedures written through `kb capture --flow` and served by `kb how`, never by `kb ask`. 1 active flow.

A flow is identified by its GOAL and its scope, not by the coordinates it touches: "place an
order" and "cancel an order" travel the same routes and are not the same procedure. It is
searched from its own index, because a procedure names the generic nouns of a whole journey
and would otherwise be a plausible answer to most questions asked in ordinary words.

| id | goal | confirmations | disputed | scope |
|---|---|---|---|---|
| [`KB-AFB2D3C5`](flows/KB-AFB2D3C5.md) | `an order placed on the storefront and read back in Admin` | 1 | no | surface=storefront-ui |
