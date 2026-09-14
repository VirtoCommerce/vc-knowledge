# How this corpus came to hold what it holds, 10–14 September 2026

This repository's git history was rewritten when it was made public, so the 33 commits that built
the corpus no longer exist as objects. They carried the reasoning — which deployment each
regeneration was taken from, why each plane holds what it holds, and what every batch of captured
facts cost to obtain. They are preserved verbatim in `docs/commits-2026-09.txt`. **Where this page
and that file disagree, that file is right.**

The workbench that produces this corpus is a separate repository, `vc-kb-lab`; its own history and a
fuller account of the measurements are in `docs/HISTORY.md` there.

---

## The corpus was created empty, on purpose

Nothing here was transcribed by hand. The derived plane is projected from a pinned deployment and
regenerated wholesale; the experiential plane is written by agents through a door, and it started at
zero **twice**.

The second start is the one worth knowing about. 37 facts had been seeded into the experiential
plane from earlier measurement rows — and all of them were cleared. An independent review put it
plainly: *"the base answers 20 of 26"* means *"the seeder's corpus agrees with the seeder's reading
of the seeder's runs 20 of 26 times"*. No amount of analysis separates those; only a different
writer does. The seeded corpus was tagged as a shadow corpus to compare against, and the plane began again with
nothing in it. **Those tags — `seed-w0.5` and `seed-w0.5-corrected` — did not survive the history
rewrite and resolve to nothing here.** A pointer that reads like a reference and leads nowhere is
the defect this corpus has a gate for; it is named rather than left to be discovered.

What was recorded at the moment of clearing, because it is the baseline everything after is measured
against: the base still declined nothing. Asked what residue a placed order leaves — a behavioural
question no contract covers — it served the derived route listing for `/api/order/customerOrders`
at exit 0. So the starting condition was never *"the base says nothing"*; it was *"the base answers
a behavioural question with a contract table"*.

---

## How the derived plane got its shape

**One entry per capability surface**, keyed on the route prefix, with the owning module carried as
data. Justified against measured demand rather than defaulted: of 15 contract questions measured,
none is answered by a single operation. A module key was rejected because five prefixes are served
by more than one module, and in two of those the split separates a reference-set answer from the
question that asks it.

Four things the first sweep found that no design document recorded:

* `/docs/PlatformUI/swagger.json` **is a merged document** — all 825 coordinates appear in it a
  second time. A per-module sweep trusting the document name would double the surface.
* `operationId` is **not unique**: 816 distinct ids over 825 coordinates.
* `Subscriptions` is plural too, not only `Mutations`. Root type names are read from introspection,
  never assumed.
* A module record's `platformVersion` is the version it was **built against**, not the one running.

**The id is `sha256(subject)` truncated to 8 hex digits.** Two people's agents have no shared
counter, so a sequence is wrong the moment there is more than one writer — and a timestamp is worse,
because two agents recording the *same* fact would mint two ids and the base would hold it twice. A
knowledge base wants two independent captures of one fact to **collide**, not to avoid each other.

**Which release this describes is derived, not stated.** "The latest release build" had three
readings that differ by a lot, so the environment pins the *registry* and the extractor compares the
deployment's reported module versions against every bundle it lists. The answer for this corpus:
bundle v14, platform 3.1007.26, exact platform match, 50 of 53 comparable module versions.

**Every entry describes a type.** An early version named 89 types in signatures and described none
of them — `CartTotalType` is reachable only as `CartType.cartTotals`, so the plane mentioned it and
stopped, and a live run asked for its fields twice, was served `Query.cart` both times, and settled
it by introspecting the deployment by hand. 287 entries became 590, and ~280 KB of repeated tables
came out.

---

## The gate, working

`kb check` regenerates the corpus in memory and byte-compares. It has gone red for real, three
times, and each was upstream rather than local:

* A module's patch version moved on the deployment (`3.1001.4 → 3.1001.8`). The pin is a hash over
  the platform and module versions and is stamped into every entry, so one patch release rewrites
  the whole corpus — while the contract itself did not move at all.
* `VirtoCommerce.Catalog` went `3.1029.5 → 3.1029.6` mid-run. 1170 generated files, one line in one
  of them.
* Scalar anchors were removed: 160 entries had claimed to be about `String`, `Boolean` and `Int`.
  An anchor's job is to raise a flag when the thing it names changes, and `String` cannot change —
  those were pure claim with no mechanism behind them. They also pointed readers at
  `gql-type-string`, an entry that was never generated.

A corpus that describes a deployment has to be able to say when it has stopped describing it. The
alternative — noticing months later that an entry cites a version that was never there — is the
failure this gate exists to make impossible.

---

## Where the captured facts came from

Seven runs, each an agent given a task on a live deployment, writing back what it had to find out.

| run | what it worked on | what it left behind |
|---|---|---|
| 01 | promotions | where a cart-subtotal reward actually lands, and that promotions are re-evaluated on every cart read |
| 02 | order discount | the reward lands only in the order's own discounts; no rate is persisted anywhere |
| 03 | organization roles | the role gates **controls, not routes** — so a permission test must assert on the control and never on the nav |
| 04 | member state | three independent fields represent "blocked", written by different doors |
| 05 | invitation lifecycle | unblocking an invitation is not an inverse; a destroyed invitation cannot be re-sent and the address stays taken |
| 06 | shared lists | `scope` has a third value the schema documents nowhere, and it serves a whole list to a caller with no account |
| 07 | order fields | a variation on an order records nothing about its parent; a payment is for one amount and totals another |

Two properties of that list are worth more than any single entry in it.

**They are mechanisms, not instances.** None names an account, an order or a price. That was the
distinction that cost the discarded seeded corpus eight of forty-five rows.

**They correct themselves.** Run 05 wrote an entry recommending a remedy, disproved it twenty
minutes later, and replaced it with `kb supersede` — six hours after that verb existed, with nobody
having told it the verb was there. Run 06 did the same within twenty minutes of writing. Run 07
declined to confirm an entry it had been served, because its order carried no discount and that
entry's claim was therefore not observable — *it took the shape and found it somewhere else.*

---

## What an entry can and cannot say about itself

The confirmation count, the disputed flag and the versions a fact holds on are **computed from
`evidence[]`**, never declared. An entry can only say it holds where something actually looked.

A capture landing on a fact the base already holds is **refused, never merged**. Whether two claims
about one coordinate under one scope agree is exactly what measurement showed text cannot be asked —
one pair stating a single fact scored 0.00 on wording similarity. The writer says which, with
`confirm` or `dispute`.

A contested fact **stays served**, at trust level `disputed`, with both sides in the delivered
block. Hiding a contested fact hides the contest.

An observation made on a deployment other than the one the derived plane describes **says so at the
point of use**, and is never filtered: where no contract covers a coordinate, an observation from
somewhere else is still the best the base has. One confirmation on the reference deployment clears
the marker.

Since 14 September an evidence row carries the **pin and platform version**, read out of
`derived/pin.json`. Before that, 54 of 54 rows carried a deployment name and nothing else — and
this repository's own README says an environment's name is not evidence of anything.

---

## Known limits, stated rather than discovered later

* The registry's `contentHash` covers the index that names the bundles, not the manifests it points
  at. A movement in those is invisible until an extraction re-reads them.
* A GraphQL fact **cannot be labelled at a module version at all** — introspection publishes no
  attribution of a schema field to a contributing module. What can honestly be said is the platform
  version serving the schema, plus, on a deployment running pre-release modules, that any of them
  could be a contributor and none can be ruled out.
* `KB-27B4CD10` and `KB-4D082C89` disagree about what a pending invitee looks like. The fingerprint
  gate was right to pass them — different anchors, different scope, two distinct facts. One clause
  inside an otherwise-correct entry went stale, and nothing in a text corpus detects that.
* Two anchors are somebody's guess at what a button called:
  `Mutations.deleteOrganizationContact` (the schema offers `deleteContact` and
  `removeMemberFromOrganization`) and `Promotion.isActive`. Replacing a guess with another guess is
  how an observation becomes an assertion, so they stand until somebody observes them.
* No configurable product exists on this deployment. Two demand rows stay open because of it, which
  is the base recording a gap it knows it has.
