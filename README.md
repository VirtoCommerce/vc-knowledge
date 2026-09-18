# vc-knowledge

The knowledge base the agentic-QA tooling in
[`vc-mcp-testing-module`](https://github.com/VirtoCommerce/vc-mcp-testing-module) reads and writes.

One base, at the root of this repository. See *What used to be here* below.

## What is in it

| path | what |
|---|---|
| `kb.json` | the manifest: the id rule, and which index file serves which plane |
| `index.json` | one row per entry — everything needed to RANK an entry, nothing needed to read one. One fetch answers "what does this base hold?" |
| `entries/KB-XXXXXXXX.md` | one fact each: frontmatter (subject, question, anchors, scope, `evidence[]`) plus the claim in prose |
| `log/YYYY-MM-DD/*.jsonl` | one file per session: what was asked, what matched, what missed, what was captured or confirmed |

An entry records something an agent **saw on a deployment** — not something it read in the source, and
not something it inferred. `evidence[]` carries one item per independent observation, with the
deployment and the timestamp; the confirmation count and the disputed flag are computed from it and
never declared, so there is only one copy of how well attested a fact is.

A **dispute never retires anything.** One contradicting observation against four confirmations is a
flag, not a deletion: the entry is returned with both sides and a person decides.

For the counts, run `kb stat`. They are not written here, because a number transcribed into a README
is correct exactly once.

## How to read it

```
npm run kb -- ask "what does the storefront members Active column reflect?"
npm run kb -- show KB-27B4CD10
```

Reading needs **no credential** — the repository is public, and someone with no write access is a
full-value reader.

`ask` distinguishes *"the base was read and holds nothing on this"* from *"the base could not be
read"*, and that distinction is the point of the tool: the first is work to do, the second is a
retry. Collapsing them is how an agent that could not reach the base concludes that nothing is known
and then writes a duplicate of an entry that already exists.

## How it is written

**Only by the tool, and only as one atomic commit per session.** An agent's `capture`, `confirm` and
`dispute` queue locally and send nothing; at session end the push re-reads the current head, the
current `index.json` and the current body of every entry it is mutating, then lands entries, index
and log together — blobs → tree → commit → ref compare-and-swap.

That shape is not incidental. Writing one file per call would allow a state where `index.json` names
an entry that is not there, and patching a *cached* index would silently drop whatever another
session committed in between.

**Do not hand-edit `index.json`.** It is materialised from the entries; a hand-edited index is the
classic drift source. If it ever disagrees with `entries/`, `kb reindex` rebuilds it.

**A push may write `index.json`, `entries/**` and `log/**`, and nothing else.** That is checked on
every push, not only the first, against normalised paths — so something that merely looks like it is
inside the base does not pass.

## The log is public, and questions are stored verbatim

This repository is public and the `question` field is not redacted. That is deliberate: a hashed
question makes the miss report worthless, and the miss report is the reason to keep a log at all. It
is bounded by what this base is for — questions about the **native VirtoCommerce platform**, whose
behaviour is already public. Before anything is sent, a gate scans every queued line for the actual
values of the operator's credentials and drops any line carrying one.

**Extending this base to client deployments has to re-decide that first.** A client asks questions
about client systems, and a public log would be wrong there.

## What used to be here

Until 2026-09-18 this repository held two knowledge bases: this one, under a `v2/` prefix, and the
corpus of an earlier and larger design at the root — `captured/`, `derived/`, `rules/`, `flows/`,
`knowledge/`, `sources/`, their catalogs and its own `kb.json`. About 5.9 MB across 1,539 files,
against 234 KB in `v2/`.

Two bases in one repository, each with a manifest declaring a different schema, leave a reader —
human or agent — no way to tell which is authoritative, and the front page described the other one.
So the older corpus was moved off `main`, and this base was lifted out of `v2/` to the root, where a
base with no rival belongs.

**Nothing is lost.** The full previous tree is preserved on the **`archive/kb-v3-2026-09-18`**
branch, at the commit this one descends from, and its tool is open at
[`vc-mcp-testing-module#298`](https://github.com/VirtoCommerce/vc-mcp-testing-module/pull/298). The
89 active entries of the old `captured/` plane were migrated into this base beforehand; the 12
retired ones were not, and stay on the archive branch.
