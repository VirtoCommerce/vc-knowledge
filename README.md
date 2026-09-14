# vc-knowledge

The knowledge base for Virto Commerce. Created empty, on purpose.

Three planes, and the differences between them are the whole design.

The **derived** plane is projected from a running deployment and *regenerated*, so it cannot rot and
nothing may hand-edit it. The **experiential** plane holds what an agent learned by doing, is
*written* through a door that enforces the shape, and has a lifecycle. The **flow** plane holds
procedures — how to get something done, in order — written through the same door and served by a
different verb.

The first two are apart because a regenerated corpus that is byte-gated and a written corpus that
grows cannot share a file without one of them breaking the other's gate on every change.

The third is apart for a measured reason. A procedure names the generic nouns of a whole journey —
cart, order, payment, product, search — so in one ranked list with facts it is a plausible answer to
most questions asked in ordinary words: one flow entry, written as an ordinary capture, cleared the
relevance floor on **18 of 34** replay rows where four comparably long *facts* cleared 6–7, and took
first place on questions it had nothing to do with. It is not a length effect and no threshold
reaches it. A separate index would not have been enough either — the first two planes already have
separate indexes and still compete, because `kb ask` merges both by score. What removes the
competition is a separate **question**: `kb ask` asks what is true and never sees a flow, `kb how`
asks what to do and sees nothing else.

| path | plane | what |
|---|---|---|
| `kb.json` | — | namespace, id rule, identity rule, and the shipped field set |
| `derived/` | derived | the projected plane: regenerated wholesale, byte-compared, never edited |
| `derived/entries/` | derived | generated entry files, one per capability surface |
| `derived/rest/`, `derived/graphql/` | derived | the contract tables those entries cite |
| `derived-index.json` | derived | generated retrieval index |
| `derived-catalog.md` | derived | generated one-line-per-entry digest |
| `captured/` | experiential | entries written by agents through `kb capture` |
| `captured-index.json` | experiential | retrieval index over the **active** captured entries |
| `captured-catalog.md` | experiential | one line per captured entry, retired ones included |
| `flows/` | flow | procedures written through `kb capture --flow`, served by `kb how` |
| `flows-index.json` | flow | retrieval index over the **active** flows |
| `flows-catalog.md` | flow | one line per flow, retired ones included |

`kb extract` wipes and rewrites everything on the derived plane. It never touches `captured/`, and
`kb check` never compares it — a capture must not be able to fail a gate that exists to police the
contract.

## Which deployment this corpus describes

`vcptcore-stable`. **Which release that is, this file does not say** — it is read, and the answer
is in `derived/release.json`, regenerated with the rest of the corpus.

Naming the release here would be a copy of a value that has a publisher. The registry moves without
us, so the copy would drift and nothing would notice. What is pinned instead is the **source**:
`RELEASE_REGISTRY` in the environment file points at `VirtoCommerce/vc-modules/bundles/stable.json`,
the extractor follows every bundle it lists, compares each against the module versions the
deployment actually reports, and records what it found — including where that release could be
obtained from, read out of its own manifest (platform image and tag, module feeds, frontend theme).

Two consequences worth knowing:

* **The registry publishes no tags and no releases**, so its URL necessarily names a branch. That
  is recorded as `registry.refIsMovable`, with a hash of the bytes that were read, so an
  identification stays auditable after the branch has moved.
* **`kb check` will fail when upstream publishes a new bundle**, because the identification is part
  of the corpus and it changed. That is the alarm working, not a fault: the reference release moved
  and the corpus should be re-extracted or the pin reconsidered.

The environment's *name* is not evidence of anything. This one is called `stable`, and the
comparison found it running a bundle one whole release behind the current stable one.

## What populates it

The `kb` tool, which lives in the workbench (`vc-kb-lab`), not here. It reads what a pinned
running deployment publishes about itself — `GET /docs/{ModuleId}/swagger.json` per module and
`POST /graphql` introspection — and writes this repository. Nothing is transcribed by hand.

```
kb extract --env localhost      # regenerate the derived plane from a deployment
kb check                        # regenerate in memory and byte-compare; a difference fails
kb ask "how do I get a token"   # resolve across both planes, returning the answer contract
```

An extractor that cannot reach a deployment reports **SKIPPED** and exits non-zero. It never
reports a pass.

## What makes two records the same fact

The question the experiential plane had to settle before it could accept a second writer, because
two agents recording one finding in their own words is the normal case, not the edge case.

**Two records are the same fact when their normalized anchors and their scope axes agree.** The
claim's wording is deliberately not part of the test.

That is a measured result, not a preference (`docs/adr/measurements/kb-dedup-2026-09` in the QA
repo). Over 19 independently-recorded pairs labelled by hand:

* the wording-similarity range of pairs that **must** collapse (0.29–0.75) **contains** the range of
  pairs that must **not** (0.31–0.44), on both the question and the answer text — so no threshold
  separates them;
* one pair stating the same fact — an admin bearer token from the password grant — scored **0.00**,
  so wording does not even raise the candidate;
* what separated the pairs that had to stay apart was the **scope**: same endpoint, same grant,
  different principal, opposite required fields.

So the coordinates raise the candidate and the scope decides. `POST /connect/token` appeared in the
demand rows as four different strings, so anchors are normalized (scheme and host stripped, a
leading `{BACK_URL}` stripped, `<id>`/`{id}`/`:id` collapsed to one parameter) before comparison.

**When a capture lands on a fact the base already holds, the door refuses and names it.** It does
not merge: whether two claims about one coordinate under one scope agree or contradict is exactly
what the measurement showed text cannot be asked. The writer says which:

```
kb confirm <id> --deployment <env>                    # raises the count, writes no second entry
kb dispute <id> --deployment <env> --note "…"         # both sides stay, the entry is served as disputed
```

Nothing declares a confirmation count, a disputed flag, or the versions a fact holds on. All three
are read out of `evidence[]`, so an entry can only say it holds where something actually looked.

## Does an entry carry `kind`?

**No.** §13.3 lists it, `SCHEMA-COMPARISON.md`'s MVP cut drops it, and the two had to be settled
here because this is the first step that would write the field.

Decided out, for reasons that are on the record rather than inherited: `KIND-ANALYSIS.md` measured
1 writer of 10 asking for it, `decision` and `convention` scoring zero across five task shapes,
`behavior` absorbing 58% of the corpus, and a third of the corpus mapping to none of the seven
values. The deferral's own release condition is *retrieval* evidence, which this step does not
produce — so shipping the field now would ratify a vocabulary on no evidence, and a mislabelled
entry is sticky in a way a missing field is not. The cost of the decision is one migration pass if
retrieval evidence later asks for it, which is cheap next to a vocabulary nobody can correct.

What `kind` was mostly proxying — generated versus learned — is already carried by `plane`.

## What one entry is about, and why

The open question this step had to settle. One entry per `operationId` would give 825 entries on
this deployment (1,650 operation instances, halved once the aggregate document's copies are
deduplicated) — and step 0 measured **zero** questions that one operation answers.

**One entry is one capability surface**, keyed on the **route prefix**, with the owning module
carried as data rather than in the key.

Justified against the 15 `CONTRACT` rows step 0 recorded, not defaulted:

| what was asked | n | what answers it |
|---|---|---|
| how a token is obtained | 9 | one coordinate — `POST /connect/token` and its form fields |
| an xAPI signature, its required input fields, its response fields | 4 | a GraphQL root field plus the input/return types it drags in |
| which endpoint serves X, and what its response carries | 2 | a resource, with the schemas it references |
| **answered by one `operationId` alone** | **0** | — |

Why the route prefix and not the module:

* Five prefixes here are served by more than one module. `/api/pages` is four modules of one
  operation each (BuilderIO, Contentful, Pages, Sanity) — a module key turns one answer into four
  entries an asker cannot choose between. `/api/catalog` carries three `Pricing` operations among
  94 `Catalog` ones, and those three are the answer to reference-set #11; a module key files them
  away from the question that asks for them.
* The asker knows the noun, not the assembly. Ten module ids here are substrings of another
  (`Payment` / `NativePaymentMethods`, `Customer` / `CustomerExportImport`), so a module name is
  not a key a question can be phrased in.
* The platform's own grouping agrees. None of its 60 tags is named after a module — they are
  capability nouns (`Shopping Cart`, `Order Management`) — and 42 of the 60 map to exactly one
  route prefix. The route prefix is that grouping in machine-stable form.

The rule, stated so it can be checked: the key is the first two non-parameter route segments,
deepened to three for any group above 25 operations. GraphQL keys on the root field. A module whose
document declares zero paths gets one entry recording that as a fact.

**What this deliberately does not do.** It does not author question-shaped entries. A real question
("which endpoint lists the payment methods a store has enabled") cannot be derived from a contract
by a zero-LLM generator — only a template over a coordinate can, and that is a coordinate with a
question mark rather than a question. Writing the real ones is authoring a bridge, which is an
experiential entry and a later step.
