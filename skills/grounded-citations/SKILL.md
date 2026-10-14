---
name: grounded-citations
description: "Ground answers and documents in cited, verifiable sources."
version: 1.2.0
author: Hermes Agent + Teknium
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [Research, Citations, Grounding, Sources, Web, Reports]
    category: research
    related_skills: [arxiv, pdf, reddit-reading, rss-feeds, youtube-content]
---

# Grounded Citations

Every claim taken from an outside source gets an inline numbered citation and a
`Sources:` list, Perplexity-style. A ledger script owns the `url → [n]` mapping
so the numbers and URLs come from retrieval, never from memory — the model only
ever emits small integers it was handed.

For high-stakes work the same ledger doubles as a fact-checking chain: verbatim
quotes are attached to each source (rejected unless they literally appear in
the fetched page text), claims from model knowledge are flagged `[unverified]`,
and `verify --evidence` fails any draft whose cited sources carry no evidence.

This skill covers answers in chat, written documents (markdown, PDF, docx,
slides), and research reports. It does not cover academic BibTeX pipelines —
for conference papers use the `arxiv` skill, which this skill
feeds (see `references/citation-formats.md`).

## When to Use

Use whenever an answer or artifact rests on information you fetched rather than
knew:

- Research, comparisons, news summaries, "what is the current state of X"
- Any deliverable you write to disk that quotes, paraphrases, or reports
  outside facts — reports, briefs, docs, decks, wiki pages
- Fact-finding where the user will want to check your work
- Multi-source synthesis where conflicting sources must be attributed

Skip inline citations when the retrieval is incidental to another task — a
quick syntax/version lookup mid-coding, casual conversation, creative writing.
Mention a URL only if the user would plausibly want the link.

## Prerequisites

None beyond the standard toolset. `scripts/sources.py` is stdlib-only Python 3.
Retrieval comes from whatever is configured: `web_search`, `web_extract`,
`browser_navigate`, or `terminal` (curl, CLIs).

Ledger location: `$HERMES_HOME/cache/citations/ledger.json` (profile-aware).
Override per task with `--ledger <path>` or `HERMES_CITATION_LEDGER`.

## How to Run

```bash
S=~/.hermes/skills/research/grounded-citations/scripts/sources.py

python "$S" reset                                  # start a clean ledger
python "$S" add https://example.com/a --title "A"  # prints: [1]
python "$S" add https://example.com/b --title "B"  # prints: [2]
python "$S" list                                   # ledger table
python "$S" render                                 # Sources: block
python "$S" verify draft.md                        # catch bad citations
```

`add` is idempotent and URL-normalized: the same page always returns the same
id within a ledger, so ids stay stable across many search/extract rounds.

## Quick Reference

| Action | Command |
|---|---|
| Fresh ledger for a new task | `sources.py reset` |
| Register a source, get its id | `sources.py add <url> [--title T]` |
| Register several at once | `sources.py add <url1> <url2> ...` |
| Register from JSON tool output | `sources.py ingest results.json` |
| Attach verbatim evidence to a source | `sources.py quote <id> --text "exact wording" --from page.txt` |
| Show ledger | `sources.py list [--json]` |
| Render the Sources block | `sources.py render [--style markdown\|plain\|footnotes\|bibtex\|evidence] [--only 1,3]` |
| Render only what a draft cites | `sources.py render --cited-in draft.md` |
| Rewrite a draft's Sources block in place | `sources.py render --replace-in draft.md` |
| Check a draft's citations | `sources.py verify draft.md [--strict] [--min-coverage 0.6] [--evidence]` |

## Repository mechanism inspections

Before evaluating an external skill, prompt framework or executable repository for adaptation, read [references/repository-mechanisms.md](references/repository-mechanisms.md). It supplies revision-bound acquisition, construction-chain inspection, semantic checks of demonstrations, packaging/licensing analysis and conditional adaptation handoffs; README claims and empty templates alone do not establish exercised effectiveness.

For this user's multi-repository mechanism research, apply these campaign defaults:

1. **Bind research to the target project.** Establish the question and target constraints before assigning repository reads; the primary result is actual mechanisms and conditional adaptation options, not merely repository descriptions or a popularity ranking.
2. **Establish the corpus.** Accept either a supplied list/inventory or bounded discovery from the research question. Reconcile required sources, distinct artifacts, duplicate origins and supporting-only leads before dispatch; later discoveries do not silently expand mandatory scope.
3. **Choose the canonical delivery.** Put complete findings in each investigation ticket body and supporting evidence in exclusive project-local files unless the owner selects another home. Preserve the ticket's question, identify local-only evidence explicitly, and request a complete publishable worker return rather than a competing narrative report. Tickets track investigations; using them does not require a Wayfinder decision interview.
4. **Inspect and transfer.** Trace relevant inputs, operations, intermediate representations, outputs and consumer behavior through substantive code/instructions, supports and examples. Retain revision-bound warrants, prerequisites, losses, alternatives and reasoned adopt/adapt/decline options for the target. Research completion is not adoption or implementation approval.

Load `dynamic-workflow` before orchestrating a substantial parallel campaign; it owns independent-unit scheduling, rolling refill and serialized integration. Read the repository-mechanisms reference before each inspection for acquisition and substantive review depth.

When this user requests exact mechanisms and a full proposal from repository research, deliver both the attributable source mechanisms and an integrated construction proposal. Preserve the requested corpus coverage, including relevant closed discussions, rather than substitute a best-of shortlist or concatenate inspection takeaways. Follow each adopted mechanism back to its actual primary instructions/code, called templates and consumer; carry its concrete operations, order, intermediate fields/schemas, examples, modes and qualifications into the proposed handoff. Earlier analyses are navigation, not a substitute for those source artifacts. Show original operation/template blocks next to their corresponding candidate instructions and each necessary change or omission; source links and abstract summaries alone do not make source-faithful reuse inspectable.

For skill/framework redesigns, honor the user's artifact-first review preference: after authoring is authorized, build directly from the actual primary procedure, called templates and consumer, and expose a complete actual skill candidate with its necessary supports early. Then integrate the remaining sources and architecture without repeating sound acquisition or inserting another summary layer. Label the checkpoint's coverage and pending checks; do not call it the complete cross-corpus proposal or delay it behind another full analysis campaign. Keep meaningful source approaches as usable modes instead of averaging them into a generic voice or uniform procedure shape. Check the selected mode's fields and ordered operations against the main procedure, output template and every consumer: a general envelope must not later flatten or replace the native body it promises to preserve. In reference-informed writing systems, capture source features faithfully and apply writing preferences as project controls at use time, because filtering during capture can erase the very differences being learned. Distinguish source behavior, proposed adaptation, actual owner correction, executed evidence and owner approval.

## Procedure

① **Reset the ledger** at the start of a task that will produce a grounded
answer or document. Skip the reset when continuing work whose ids are already
in a draft — reusing the ledger keeps the numbering stable.

② **Register every source at retrieval time.** After each `web_search` /
`web_extract` / `browser_navigate` / fetch, pass the URLs to `sources.py add`
(or pipe the raw JSON through `sources.py ingest`). Do this *before* writing
prose. Registering later, from memory, is the failure mode this skill exists to
prevent. Inspect the returned body before treating acquisition as reading: a
successful fetch can contain an anti-bot shell or only an opening/closing
window. Read the saved full text or recover the omitted passages before citing
them. Record whether the inspected evidence is a full text, selected passage,
published extract or author abstract; restrict claims to that actual reading
depth. Retain human-readable chapter, verse, paragraph or page locators and the
translation/edition used, because numbering and wording can differ across
witnesses.

③ **Write cite-while-drafting.** Place the bracketed id(s) immediately after
each sentence the source supports:

```
Ice floats because it is less dense than liquid water.[1][2]
```

- No space before the bracket; each id in its own brackets.
- Max 3 ids per sentence. Cite per sentence, not one dump at the end. In mechanism matrices and per-resource sections, place citations on the actual mechanism, evidence and limitation claims, not only on the row title or heading; heading citations identify a source but do not attach provenance to the prose beneath it. Split an overloaded preservation summary into separately supported responsibilities rather than append a large citation bundle.
- Only ids the ledger returned. Never invent an id or a URL.
- Claims from your own knowledge get no citation.
- Conflicting sources: present both readings, each with its own id.
- Quote exact figures, dates, names and excerpts as the bound source states
  them; keep editorial punctuation outside quotation delimiters and render
  supported paraphrases without quotation marks. Recheck quotations introduced
  by review corrections against the passage itself: a valid ledger anchor does
  not establish that the final prose quotes it faithfully. Flag gaps explicitly
  ("no source found for X") instead of smoothing them over.

④ **Append the Sources block** with `sources.py render --cited-in <draft>` so
the id → URL mapping is generated mechanically from the ledger, not retyped.
For non-markdown targets pick the matching `--style` and follow
`references/citation-formats.md` for placement (footnotes in docx, endnotes in
PDF/LaTeX, a Sources slide in decks, per-page source lists in wiki output).

⑤ **Verify before delivering** — `sources.py verify <draft>` exits non-zero on
unknown ids, on a Sources block that disagrees with the ledger, or (with
`--min-coverage`) on prose that is too thinly cited. Fix and re-run. For evidence
work, run `verify --evidence` on every final report and the integrated draft,
not just the worker reports. Run `--strict --evidence` separately and preserve
its exit code and warnings: strict mode also fails on uncited registered
sources. Retain failed acquisitions as audit records, remove their unsupported
capability citations, and disclose intentional uncited entries rather than
inventing quotes or calling a nonzero strict run a pass. If strict mode fails
only for intentionally unused registrations, reconcile the exact uncited set
and distinguish that completeness warning from missing cited evidence. A
qualified publication may proceed only when the governing acceptance criteria
permit that limitation; an explicit strict-pass requirement remains unsatisfied.
Keep stock-verifier results separate from diagnostic runs with adapted comparison
logic; a diagnostic pass does not satisfy the original gate. Classify each actual
failure before changing anything: repair a missing or stale Sources block from
the recorded ledger, reconcile intentionally unused component registrations, and
preserve literal identifiers while investigating identity mismatches. Do not relax
comparison rules or add token citations merely to produce a passing result.
Recheck the integrated prose after worker repairs, because corrected evidence
can invalidate an otherwise mechanically valid executive summary. Resolve each
report's exact ledger path from its recorded verifier command or explicit
handoff. If the handoff names a relative ledger, resolve it against that
worker's declared exclusive evidence base and cross-check the verification
record; do not require an absolute-path substring in the prose or select an
unrelated ledger by filename alone. Check that the resolved file exists and
contains the expected source identities before running gates, because an empty
or mismatched ledger can invalidate the check. Bind final sizing and gate
records to the fully integrated text, not an earlier worker snapshot.

After replacing proposal or handoff sections with owner-confirmed wording,
rerun both gates: status reconciliation can remove citations to still-required
records without changing the substantive claim. Compare newly uncited sources
with the prior draft and restore attribution only where the final claim is
actually supported; do not relabel a lost citation as unused discovery or add
a token citation to force a pass. For authorized split delivery sharing one
ledger, verify the complete attachment and shorter canonical body separately,
and state each target's actual result; a component's intentionally uncited
registrations do not establish missing evidence in the full deliverable.

Audit worked examples against their declared premises as well as cited claims.
Narrow unsupported conclusions rather than silently adding premises to make an
example pass; disclose any separately authorized input change, because a repaired
example must not conceal a change in what the evidence establishes.

When the task requires canonical prose in a tracker body and prohibits separate narrative drafts, verify the complete assembled body without manufacturing a report file. The verifier requires a regular-file target, so a pipe such as `/dev/stdin` can be rejected. On Linux, an anonymous `os.memfd_create` descriptor containing the body can be passed to the verifier as `/proc/self/fd/<fd>` with `subprocess.run(..., pass_fds=(fd,))`; run the evidence and strict-evidence gates separately, retain their actual outputs in an authorized verification record, and close the descriptor afterward. This is a condition-specific verification route, not permission to bypass artifact-placement rules or evidence inspection.

Before handing off a research artifact or closing its ticket, load `deliver-work` and satisfy its delivery sequence. Keep the completion summary concise while linking or attaching the complete report, including its source evidence, practical implications and limits. Verification of citations is not verification that the reader can reach the report.

⑥ **Chat answers** follow the same steps with the draft in your reply: register
sources, cite inline, end with the rendered `Sources:` list. For a short answer
you may render the block from `sources.py render --only <ids>` instead of
writing to a file.

## Multi-Platform Sweeps

"What are people saying about X" / "research X across the web" is not one
`web_search`. Fan out across source types, collect in parallel, then synthesise
with every claim attributed to the platform it came from:

| Source type | Route | What it adds |
|---|---|---|
| Open web | `web_search` → `web_extract` | official docs, articles, announcements |
| Community discussion | `reddit-reading` (`search`, `thread`) | real user experience, complaints, workarounds |
| Blogs / releases / changelogs | `rss-feeds` (`read`, `discover`) | dated primary posts, version history |
| Video | `youtube-content` | walkthroughs, demos, talks |
| Code | `terminal` with `gh search repos` / `gh search issues` | implementations, open bugs |
| X/Twitter | `xurl` (needs API access) | announcements, developer chatter |

The `reddit-reading` and `rss-feeds` skills are optional. If absent, install with
`hermes skills install official/social-media/reddit-reading` or
`hermes skills install official/research/rss-feeds` before using them.

Register every URL from every route in the ledger as it arrives (step ②). Keep
opinion and measurement apart: a Reddit thread is evidence that users *report*
something, not that it is true; pair it with a primary source or label it as
sentiment. Report per-platform coverage gaps ("Reddit search returned nothing
newer than March") rather than silently narrowing to what worked.

## Fact-Checking Mode

For work where the reader must be able to check the chain — medical, legal,
financial, safety, disputed claims, or when the user asks for fact-checking —
upgrade from citations to evidence:

① **Attach a verbatim quote per source.** After extracting a page, save its
text to a file and attach the sentence(s) that carry each claim:

```bash
python "$S" quote 1 --text "Ice is about 9% less dense than liquid water." --from page1.txt
```

The quote is rejected unless it appears verbatim in the evidence text
(insensitive to whitespace, case, and markdown markup — inline links like
`_[ERAP1](https://…)_` in extracted text match the plain prose a reader sees),
so a paraphrase or misremembered figure cannot masquerade as evidence.
Copy-paste from the fetched text; never retype. Quote the sentence as the
reader sees it — the matcher sees through the extractor's markup for you, so
you don't have to reproduce link syntax or escaped asterisks in your quote.

② **Flag model-knowledge claims with `[unverified]`.** A load-bearing claim
you could not source gets an explicit marker instead of a citation:

```
The refactor likely predates the 2.0 release.[unverified]
```

`verify --min-coverage` counts `[unverified]` sentences as covered — the goal
is declared provenance for every claim, not a citation on every sentence.
If a key claim can be checked, check it; `[unverified]` is for what genuinely
cannot be, and a fact-check deliverable dominated by `[unverified]` markers
should say so in its summary.

③ **Cross-check disputed facts against a second independent source.** When two
sources disagree, cite both readings with their own ids and quotes, and say
which you weight and why. One source is reporting; two independent sources are
corroboration.

④ **Verify with the evidence gate and render the evidence block:**

```bash
python "$S" verify report.md --evidence --min-coverage 0.5
python "$S" render --style evidence --replace-in report.md
```

`--evidence` fails the draft if any cited source has no attached quote. The
`evidence` render style prints each source's quotes beneath its URL, so the
deliverable shows claim → source → exact supporting text with nothing taken on
faith. Use `--replace-in <draft>` to rewrite an existing Sources block in place
(idempotent — safe to re-run after attaching more quotes); `--cited-in` prints
to stdout instead. Both emit the heading `## Sources` (`--style plain` emits
`Sources:`).

**What `--min-coverage` counts.** Coverage is
`sentences with declared provenance / prose sentences`. A prose sentence is a
non-empty line fragment of 4+ words after the Sources block, headings (`#`),
table rows (`|`), and fenced code are dropped; blockquote markers are stripped.
Provenance is declared by either a `[n]` citation or an `[unverified]` marker,
so a sentence carrying both counts once. Run `verify` without a threshold first
and read the `info: stats:` line to see the counts before picking a number.

## Pitfalls

- **Registering after writing.** The ledger must be populated from tool output,
  not reconstructed from the draft — that reintroduces exactly the hallucinated
  -URL risk the numbering removes.
- **Renumbering mid-task.** Never hand-edit ids in a draft. Ids are ledger
  identities; if a draft cites `[4]`, `[4]` must stay that source. Run `reset`
  only between tasks.
- **Retyping URLs into the Sources block.** Always `render`. A hand-typed URL
  is an unverified claim.
- **Citing a search snippet as if you read the page.** A `web_search`
  description supports only what it literally says. Cite the extracted page
  when the claim needs the body — `web_extract` it first.
- **Over-citing.** Three ids on a sentence is the ceiling; a citation on every
  clause makes text unreadable and hides which source carries the load.
- **Citing the ledger in code/config artifacts.** Source comments belong in
  prose deliverables and doc headers, not inside generated code.
- **Parallel subagents.** Give concurrently writing workers separate explicit
  ledgers and exclusive report/evidence paths; shared-ledger writes can race.
  Keep report citation namespaces separate. Build the integrated draft with a
  parent-owned ledger, map sources by exact retrieved URL, and reattach quotes
  with `quote --from` retained matching text; copying citations does not transfer
  evidence. Require preserved tool outputs for claimed live tests, because a
  worker's summary is not execution proof. Follow
  `references/multi-report-audits.md` for the merge and final audit procedure.
- **Quoting from a snippet instead of the page.** Evidence quotes must come
  from the extracted page text, not a search-result description — `web_extract`
  first, save the text, then `quote --from` that file.
- **Paraphrasing into `quote --text`.** The verbatim check will reject it; the
  fix is to find the actual sentence, not to reword until something matches.
- **Short verbatim clauses.** The stock quote command requires at least three
  whitespace-separated words, so a valid two-word or Chinese-script warrant
  can fail attachment despite matching its declared source range. Preserve
  that original clause and its quotation bounds; attach a minimally longer
  verbatim passage from the inspected neighboring context, recording the
  attachment's exact text and separate actual bounds. Do not insert artificial
  spaces, rewrite the source, silently widen the original warrant, or relax
  the stock gate.
- **Inclusive line-range excerpts ending in a blank line.** Compare the complete
  source range joined as text with the stored excerpt. Separate `splitlines()`
  list comparisons can reject a terminal blank source line even when the joined
  text is identical. Reconcile that representation difference without stripping
  meaningful content, changing the recorded bounds or relaxing literal equality.
- **Using `[unverified]` as an escape hatch.** It marks the rare claim that
  genuinely cannot be sourced; if most sentences carry it, the task needed more
  retrieval, not more markers.
- **Hand-editing the Sources block.** Use `render --replace-in <draft>`; slicing
  the file yourself risks a stale or duplicated block that `verify` then flags.

## Verification

```bash
python "$S" verify report.md --strict --min-coverage 0.5
```

Green means: every `[n]` in the draft exists in the ledger, the Sources block
lists exactly the cited ids with the ledger's URLs, and the cited share of
source-bearing sentences meets the threshold. With `--evidence`, each cited
source also has a retrieved quotation anchor; that does not establish that
all claims attached to the source follow from it. Before delivery, inspect
load-bearing claims against their exact passages and surrounding qualifications,
using independent review when required. Name the exact reading boundary of
coverage claims: an all-row audit of selected passages is not a full
canonical-source comparison, and a complete retained-source comparison is not
fresh reinspection or execution of every upstream repository. For exhaustive
delegated comparison, use `references/multi-report-audits.md`, section 1, to
assign and reconcile complete source reads. Report mechanical citation checks
separately from substantive support and unresolved disagreement; neither source
counts nor coverage percentages certify factual reliability. Read the warnings
even when the exit code is 0 — uncited registered sources may be unused discovery
or access attempts rather than inspected evidence. Distinguish those records
from claims that actually lost their attribution.
