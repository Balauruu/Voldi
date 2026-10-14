# Voldi: what it is and how it works today

Snapshot of `C:\ai_projects\voldi` as of 13.10.2026, written for someone who will redesign the architecture.
Based on reading the repo files (CLAUDE.md, the three skills, plans 3.0 / 3.1 / 3.2, config, scripts, industry sheet, signals data). I did not run the code or the tests, and I did not see the LeadBridge repo, so anything about those is taken from the docs only.

---

## 1. The short version

Voldi is a **pre-call research assistant for a B2B telecom salesperson** (Radu, Vodafone Business hunter in Bucharest, target firms with 5 to 49 employees, about 100 cold calls a day).

It is not a standalone app. It is **Claude Code running in a folder**, steered by `CLAUDE.md` plus three skills (markdown instruction files), with a handful of Python scripts for the mechanical parts: Pipedrive API access, parsing saved ListaFirme pages, assembling notes, and regex-based "verifiers" that check each note's format.

Its job, per company: find out what the firm actually does, find a reason to call now, and produce a short note in Romanian that Radu reads seconds before dialing. The note's single goal is **to book a meeting**.

Everything the user reads is Romanian. Code, skill names and instructions are English.

---

## 2. The surrounding system (what Voldi depends on but does not own)

```
ListaFirme (Romanian company registry, paid web platform, Radu's login)
      │  Radu sets filters (CAEN code + Bucharest + 5–49 employees)
      ▼
LeadBridge  (separate repo ../lead_bridge: Chrome extension + Python crawler)
      │  crawls N firms from a saved selection, saves each ListaFirme page as HTML,
      │  writes rows to an Excel workbook, writes a batch manifest JSON
      ▼
Pipedrive import (done by Radu)  ──►  Pipedrive CRM (leads, persons, labels, notes)
      │
      ▼
VOLDI (this repo)
```

Hard boundaries in `CLAUDE.md`: Voldi never touches LeadBridge, the ListaFirme extraction or the Pipedrive import. It reuses LeadBridge's Python venv (`..\lead_bridge\.venv`) and its saved ListaFirme pages.

External sources Voldi reads during research: ListaFirme (in Radu's logged-in Chrome, once per company, because repeated visits log him out), Termene / Risco / TotalFirme (registries), the company's own website, Google Maps reviews, eJobs (Cloudflare blocks fast access), undelucram.ro / Glassdoor, news, SEAP/SICAP tenders, and its own radar files.

---

## 3. How the work is organised (human process)

- **Radu** decides. Nothing is created, changed or deleted without his explicit "DA".
- **The architect** is a separate Claude chat. Radu brainstorms with it, it writes versioned plan files (`updates/voldi-X.Y.md`) and reviews results.
- **Voldi** (Claude Code) is the developer *and* the runtime. It reads the plan, audits it against the real files, reports differences, implements phase by phase with a DA before every writing phase, then updates the plan's "Status" section.

So the repo mixes three things: the product (skills, scripts, data), the build process (plans, handoffs, audit reports), and run outputs (radar files, lots, Excel).

---

## 4. Folder map

| Path | What it is |
|---|---|
| `CLAUDE.md` | Top-level rules: flow, approvals, language, boundaries. Says `voldi-3.0.md` is the active plan. |
| `.claude/skills/radar/SKILL.md` | Weekly industry radar (prompt-only skill). |
| `.claude/skills/hunting/SKILL.md` | Per-company call note from Pipedrive leads labelled `research` (prompt-only skill, ~23 KB of rules). |
| `.claude/skills/pipedrive-note/SKILL.md` | Manual, single-lead note writer with per-note confirmation. |
| `.claude/settings.json` | Tool permissions: Pipedrive `addNote` / `updateLead` allowed; `updateNote` and deletes denied. |
| `config/hunting-mode.md` | One switch: `write_mode: on/off` (currently **on**). |
| `config/pipedrive-field-map.md` | Pipedrive custom field hash keys and lead label IDs. |
| `config/product-blocklist.md` | Words that must not appear in a hunting note (brands, Vodafone product names). |
| `catalog/nonstandard-solutions.md` | ~106 solution cards (S01–S106), each with a status `?-E` (to confirm with the solutions colleague) or `OK-E` (confirmed; only S01). |
| `catalog/standard-solutions.md` | Empty placeholder. |
| `catalog/subscription-tech.md` | "Technology as a subscription" categories. |
| `catalog/archive/` | Retired problem/signal catalogs. Never read. |
| `radar/radar-YYYY-MM-DD[-n].md` | Radar outputs, append-only. |
| `industries/distributie-angro.json` | "Industry sheet" for wholesale (CAEN 46): 7 typical pains (P1–P7) with job-ad regexes, 9 subsectors, solution lists, question templates, money templates. |
| `lots/distributie-angro/` | One batch: manifests (`lot50.json`, `lot70.json`), `signals.json` (research facts per CUI), `config.json` (group subsidiaries, subsector overrides, duplicates), small helper scripts. |
| `lots/distributie-angro_2026-10-12.xlsx` | Output Excel for that batch. |
| `scripts/pipedrive_*.py`, `refresh_field_map.py` | Thin Pipedrive REST client (token in `.env`), read, add-only write, field map refresh. |
| `scripts/fetch_listafirme_page.py` | Reuses LeadBridge's Chrome session to fetch a ListaFirme page. |
| `scripts/verify_note.py` | Regex verifier for the **hunting** note format. |
| `scripts/build_lot.py` + `scripts/lead31/` | The **3.1 batch pipeline**: profile parsing, note assembly, group detection, verifier, catalog loader. Produces Excel. |
| `tests/` | unittest suites for the client, verifiers and lead31. |
| `updates/` | Plans: 2.1–2.5 (history), 3.0 (declared active), 3.1, 3.1-HANDOFF, 3.2 (newest, diagnosis only). |
| `docs/superpowers/` | Older specs/plans from a planning plugin. |

---

## 5. The three intended phases (from plan 3.0)

### Phase 0: Radar (skill `radar`)
"Which industries deserve calls now?"
- Web-searches at least 12 industries for **external pressure**: laws, deadlines, funding calls, scandals, big projects, hiring waves.
- Returns 4–6 industries, each with: fact + date + link `[F]`, probable pressure on firms `[I]`, why now, which small firm types to call, and a "read this" line (official act + one press article).
- Rules: primary sources first, drafts marked as not adopted, only new items vs. the last 8 weeks of radar files, "NONE FOUND" is allowed (never pad the list), **zero Vodafone products**.
- Saved automatically to `radar/`, never overwritten. The only file Voldi writes without asking.

### Phase 1: Hunting (skill `hunting`)
"Why call this company now, and what to say?"
- Input: every Pipedrive lead carrying the `research` label. The Pipedrive connector has no label filter, so it pages through all leads (100 per page) and filters locally.
- One company at a time: identity check (CUI vs. name) → Pipedrive data (person, phones, existing notes, a `[CIFRE-LEADBRIDGE]` figures block if under 90 days old) → registries for the person's role → ListaFirme once → website, news, tenders, job ads.
- Classification, no numeric score:
  - **HIGH**: a dated company trigger under 6 months old (news, tender, new location, job ad).
  - **MEDIUM**: no trigger but at least one *specific* fact from the firm's own site or a public page under 2 years old (registry data does not count). That fact becomes the hook.
  - **LOW**: nothing specific. Short note, minimal research.
- Note format (Romanian, fixed): priority line, who to call + what probably interests them, business (what it does, how it makes money, size), why now (trigger / industry context from the radar), hook, a 4-line scripted opening, 3 fallback questions, what to find out, sources.
- Hard rules: every fact has link + date; hypotheses tagged; **no products, solutions, prices or financing**; no day/time in the invitation; no unsourced claims in the opening.
- Then `verify_note.py` runs on the note (one correction round allowed). Pass → `addNote` to the lead and swap label `research` → `researched - high/medium/low`. Fail → label `researched - blocked`, no note.
- Write rules: add-only. Never edit or delete notes or other fields.
- Measured speed in trials: about 7.5 minutes per company. 171 leads were waiting at the time (estimated ~18 hours).

### Phase 2: Dossier (documented only, **not implemented**)
After a meeting is booked ("fă dosar pe X"): preliminary SWOT, a qualification grid (PQP: need, fit, buying center, budget, urgency, competition, resources, timing, potential, coach, each marked + / − / = / ?), 5 meeting questions each mapped to the hypothesis it tests, and directions to explore using the solution catalog. Plan 3.0 said "implement in 3.1", but 3.1 went a different way (next section).

### Manual writer (skill `pipedrive-note`)
Writes one dictated or finished note to one named lead/deal, after showing it and getting "da". Refuses bulk.

---

## 6. What actually happened next: the 3.1 batch pipeline

Plan 3.1 (12.10.2026) introduced a **second, different pipeline**, aimed at volume: Radu calls in industry blocks (50 firms in the morning from one industry, 50 in the afternoon from another) and wanted "script-first, Claude only where unavoidable".

Flow:
1. LeadBridge was extended: per-industry saved selections, extract only N new firms, saved position per list, a batch manifest JSON.
2. `lead31/profile.py` parses the saved ListaFirme HTML: CAEN, administrators, shareholders with stakes, displayed phone numbers, employees, revenue, branches.
3. Claude does the web part in Radu's Chrome (Google Maps site link and reviews, eJobs, website reading via WebFetch) and writes short facts into `lots/<industry>/signals.json` (per CUI: reviews, jobs, site facts with optional pain tag P1–P7).
4. `build_lot.py build` assembles a note per firm from: profile data + industry sheet templates + signals + catalog, runs `lead31/verify.py`, and writes an Excel (one column per block, full note, empty "Status CRM" and "Call note" columns). **No Pipedrive import.**

The 3.1 note has blocks 0–5: who you are talking to (with alerts if the contact is not the administrator), the firm, weak reviews in the last 2 months, recent job ads, "where it loses money/time" (marked HYPOTHESIS), and **1–2 recommended solutions from the catalog** plus 2–3 discovery questions.

Result: 70 wholesale firms processed, 70/70 notes "passed" tests.

### Plan 3.2 (13.10.2026, diagnosis only, nothing changed yet)
Radu audited the 51 notes he used and found them generic. Voldi's own diagnosis:
- **No sentence in the note is written for that specific company.** Claude only writes short facts into `signals.json`; everything else is Python templates plus the industry sheet.
- 40/51 notes fell back to the generic subsector pain. Questions came from 4 templates. Glued sentences read badly ("On the site it says that service at X…: how do you…").
- Logic bugs: a pure intermediary (no stock) got stock questions; alcohol flagged as perishable; the secondary solution copied the primary's pain; the job-ads block printed the reviews source text (49/51); legal entities not detected as contacts; group subsidiaries still got ERP proposals; group duplicates not merged.
- Signal yield was near zero: 0 useful reviews, 3 job ads across all firms, 22 "no site found" without a log of what was searched.
- **The tests checked presence of blocks, not quality**, and even whitelisted the template openings.

3.2 proposes: mechanical fixes, then flip the split (script only collects data into a per-company JSON; Claude writes every sentence of the note into `notes/<CUI>.json`; Python only assembles), deeper per-company source collection with a search log, a new shorter note structure, quality tests, and a blind 10-note human review with an 8/10 pass bar.

---

## 7. How the pieces map to "script vs. model"

| Concern | Who does it today |
|---|---|
| Lead sourcing | LeadBridge (outside Voldi) + Radu's ListaFirme filters |
| Selecting what to research | Pipedrive label `research` (3.0) or a LeadBridge batch manifest (3.1) |
| Registry facts (CAEN, people, phones, size) | Python parser on saved HTML (3.1) or Claude reading ListaFirme/registries (3.0) |
| Web research (site, Maps, jobs, news) | Claude via Chrome automation + WebSearch/WebFetch, one company at a time |
| Reasoning (trigger, priority, hypothesis, opening) | Claude, guided by long skill files (3.0); mostly templates (3.1) |
| Format/content guardrails | Regex verifiers: `verify_note.py` (3.0) and `lead31/verify.py` (3.1) |
| Persistence | Pipedrive notes + labels (3.0), Excel + JSON files (3.1), markdown radar files |
| Safety | `settings.json` permission allow/ask/deny, `write_mode` switch, "DA" culture |
| State between sessions | Pipedrive labels, files on disk, plan "Status" sections. No database by rule. |

---

## 8. What I see as the real architectural problems

These are my reading of the repo, for your friend to challenge.

1. **Two pipelines with opposite philosophies live side by side.** 3.0 hunting says "no solutions or products in the note, ever" and blocks product names with a verifier. 3.1 puts catalog solutions in block 5 of every note. `CLAUDE.md` still declares 3.0 active and does not mention `lead31/`, `build_lot.py`, `industries/` or `lots/` at all. A new session gets contradictory instructions depending on what it reads first.
2. **Two verifiers, two note formats, two outputs** (Pipedrive vs. Excel). No shared schema for "a researched company".
3. **The value is in per-company insight, but the cheapest parts got automated and the expensive part got templated.** 3.2's own conclusion is that templating killed the specificity that makes a call note worth reading.
4. **Throughput vs. depth is unresolved.** Target is ~100 calls/day. Deep research takes ~7.5 min/company with Claude driving a browser, and sources like eJobs, LinkedIn and Facebook block automation. The current design has no explicit tiering (for example: cheap registry triage on all firms, deep research only where a signal exists).
5. **Rules live in very long prose files** (the hunting skill is ~23 KB). Every rule change is a plan, an audit and a rewrite. Behaviour depends on the model reading and obeying all of it every run.
6. **Tests check form, not usefulness.** Both verifiers passed notes Radu would not call with. The only real quality signal is Radu's own judgement, and there is no feedback loop from call outcomes back into the system (the FEEDBACK block was removed; "Call note" column is filled manually and never read back).
7. **Fragile coupling to the user's environment:** Radu's logged-in Chrome, ListaFirme session limits (one visit per company or he is logged out), LeadBridge's venv and saved pages, Windows paths, Pipedrive connector quirks (no label filter, ~75k-character pages).
8. **Process overhead is part of the product.** Plans, handoffs, audits and status logs sit next to runtime files, and some runtime rules exist only inside plan Status sections.
9. **Open items not closed:** dossier phase not built; `standard-solutions.md` empty; almost all catalog cards are "to confirm"; farming (existing client base) planned but not designed; the curățenie/pază industry sheet not started.

## 9. Questions worth answering before the redesign

- What is the unit of value: a note Radu reads in 10 seconds, a ranked call list, or a booked meeting? Which one should the system optimise and measure?
- Should research depth depend on a cheap first pass (registry + "has a website?" + "any signal?"), so deep work goes only to firms likely to be HIGH/MEDIUM?
- One canonical company record (JSON schema) that every stage reads and writes, with Pipedrive and Excel as views of it?
- Where do solutions belong: never before the meeting (3.0) or as a hint for the opening question (3.1)? This decides most of the note design.
- How do call outcomes (connected, refused, meeting) flow back so the system learns which hooks and industries work?
- Which steps truly need an LLM with a browser, and which could be APIs, cached data or plain scripts?
- Hard constraints to keep: no bypassing logins/paywalls, add-only CRM writes, explicit approval, Romanian output, no runs on the Vodafone laptop.

---

## Appendix A: label IDs and switches (Pipedrive)

| Label | Role |
|---|---|
| `research` | input for hunting |
| `researched - high / medium / low` | output by priority |
| `researched - blocked` | note failed verification, not written |
| "de sunat azi" | Radu's own "call today" label |

`config/hunting-mode.md` → `write_mode: on` (automatic add-only writes are enabled).
`.claude/settings.json` → `addNote`, `updateLead` allowed; `updateDeal`, `updateOrganization`, `pipedrive_write.py` ask; `updateNote`, `delete*` denied.

## Appendix B: commands (Windows, from the voldi folder)

```powershell
..\lead_bridge\.venv\Scripts\python.exe scripts\pipedrive_read.py --source leads
..\lead_bridge\.venv\Scripts\python.exe scripts\verify_note.py nota.md
..\lead_bridge\.venv\Scripts\python.exe scripts\build_lot.py build --manifest lots/distributie-angro/lot70.json --industry distributie-angro --signals lots/distributie-angro/signals.json [--only CUI,CUI] [--print]
..\lead_bridge\.venv\Scripts\python.exe -m unittest discover -s tests -p "test_*.py" -v
```

## Appendix C: what I could not confirm

- The LeadBridge code and its current behaviour (not in this folder).
- Whether all tests pass right now (I did not run them).
- Git state and which changes are committed.
- Whether hunting has actually written notes to Pipedrive since `write_mode` was switched to `on`; plan 3.0's Status section does not record that switch.
