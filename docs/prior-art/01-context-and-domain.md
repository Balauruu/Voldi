# 01. Context and domain

## The user and the job

- **Radu**: junior B2B "hunter" at Vodafone Business Romania (through a partner company), based in Bucharest, started late July 2026.
- He gets a base of existing clients (farming) and also prospects new ones (hunting). Voldi currently covers **hunting only**.
- Target segment: companies with **5 to 49 employees** (from the Vodafone Account Management course). Plan 3.1 narrows it to Bucharest (+ Ilfov).
- Volume target stated in plan 3.1: **about 100 cold calls per day**, organised in **industry blocks**: 50 firms in the morning (10:00 to 12:30) from one industry, 50 in the afternoon from another. Industry selection criterion: "the owner runs the company, he does not work in it" (sits in an office, can take a phone call).
- The only goal of a hunting call is to **book a meeting**. Solutions are explored later, with the solutions colleague (Eduard, "DSC") who confirms whether Vodafone can deliver something.

## Why Voldi exists

Before a cold call Radu needs, in a few seconds of reading:
1. who he is talking to (is the contact the decision maker?),
2. what the company really does,
3. a concrete, sourced reason to call now (a "trigger" or a specific fact),
4. a short opening and fallback questions.

Doing this by hand for 100 firms a day is not possible. Voldi automates the research and writes the note.

## Hard principles that run through every version

| Principle | Where it shows up |
|---|---|
| Fact vs. hypothesis. Every fact has a link and a date. | Tags `[F]` / `[I]` / `[?]` (hunting), `IPOTEZĂ` (lot pipeline), verifiers |
| Never invent. If unverifiable: "Nu pot confirma". | CLAUDE.md, every skill |
| No prices, installments, contract lengths, eligibility or financing in any note. | `verify_note.py`, `pipedrive-note` block check |
| Public sources only. Never bypass a login or paywall; a blocked source is written as "blocat". | Skills, plan 3.2 §3 |
| Nothing is created, changed or deleted without Radu's explicit "DA". Only exception: radar files. | CLAUDE.md |
| Pipedrive writes are add-only (new notes, label changes). Never edit or delete. | `.claude/settings.json` deny list, hunting skill |
| No numeric score, no database, no multi-agent setup. | CLAUDE.md "System boundaries" |
| Never touch LeadBridge, the ListaFirme extraction or the Pipedrive import. | CLAUDE.md |

## Glossary

| Term | Meaning |
|---|---|
| **CUI** | Romanian company tax ID. The primary key for a company everywhere in Voldi. |
| **CAEN** | Romanian activity classification (NACE equivalent). Rev. 2 and Rev. 3 coexist; Voldi keeps the code as shown, never converts. CAEN division 46 = wholesale. |
| **ListaFirme** | `platform.listafirme.ro`, paid registry platform (administrators, shareholders, phones, financials). Radu's session gets logged out if a page is opened repeatedly, hence the "one visit per company" rule. Logged-out signs: "Prima oară pe site?" or masked phones like `0721-613.9 - -`. |
| **Termene, Risco, TotalFirme, Targetare** | Other registry sites used as fallbacks. |
| **LeadBridge** | Radu's separate tool (`C:\ai_projects\lead_bridge`): Chrome extension + Python crawler that extracts firms from a ListaFirme saved selection into an Excel workbook and saves each company page as HTML. |
| **`[CIFRE-LEADBRIDGE]` block** | Text block written by LeadBridge into the Pipedrive custom field "Observații": `Extras: ZZ.LL.AAAA`, employees, secondary offices, founding year, revenue/profit, active phones. Hunting uses it only if under 90 days old and fully parseable. |
| **Pipedrive** | Radu's CRM. Companies arrive as **leads** with a person (decision maker), an organization and labels. |
| **Label `research`** | Input queue for hunting. Output labels: `researched - high/medium/low/blocked`. |
| **Trigger** | A dated company event, max 6 months old: news, tender won, new location, job ad. |
| **Specific fact** | A fact about this company that would not fit any other company in the list, from its own current site or a public page max 2 years old. Registry data never counts. |
| **HIGH / MEDIUM / LOW** | Hunting priority classes (see [04-hunting.md](04-hunting.md)). No numeric score. |
| **Radar** | Weekly scan of all industries for external pressure (laws, deadlines, funding). |
| **Dossier** | Meeting preparation (SWOT + PQP). Specified, not built. |
| **PQP** | Qualification grid from the Vodafone course: need, solution fit, buying center, budget, urgency, competition, resources, timing, potential, coach. Signs `+ − = ?`. |
| **Solution card `Sxx`** | Entry in `catalog/nonstandard-solutions.md` (S01 to S106). |
| **`?-E` / `OK-E`** | Card status. `?-E` = to ask Eduard (the solutions colleague) whether Vodafone can deliver it. `OK-E (date)` = confirmed by Eduard; only Radu sets it. Today only **S01 (ERP)** is `OK-E`. |
| **DSC** | The internal solutions team / Eduard; destination for non-standard requests. |
| **Industry sheet** | `industries/<slug>.json`: typical pains, subsectors, solution IDs and question templates for one industry (3.1). |
| **Lot** | One batch of firms from a LeadBridge crawl, processed by the 3.1 pipeline into one Excel file. |
| **Signals** | Per-company web research results stored in `lots/<industry>/signals.json` (site facts, reviews, job ads, news). |
| **Architect** | A separate Claude chat where Radu designs changes and writes plans. |
| **DA** | Romanian "yes". Explicit approval required before any write phase. |
| **caveman** | A terse-output skill; allowed for internal work only, never in a note. |
| **write_mode** | Switch in `config/hunting-mode.md`; `on` lets hunting write to Pipedrive. |
