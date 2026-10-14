---
name: to-tickets
description: "Turn plans into approved, dependency-aware work tickets."
version: 1.0.0
author: Matt Pocock, Balauru, Hermes Agent
license: MIT
platforms: [linux, macos, windows]
disable-model-invocation: true
metadata:
  hermes:
    tags: [planning, tickets, dependencies, vertical-slices]
    related_skills: []
---

# To Tickets

Turn a plan, spec or conversation into fresh-context, tracer-bullet tickets with explicit blocking edges. Draft and obtain human approval before authorized publication; publication does not implement, claim or complete the work. This is a prompt workflow using Hermes tools, not a tracker installer.

## When to Use

- Invoke `/to-tickets` to decompose work or reconcile an existing implementation backlog. The retained invocation flag is compatibility metadata, not an enforced Hermes explicit-only gate.
- Don't use for implementation, speculative architecture decisions or automatic parent closure.

## Inputs and prerequisites

- **Source:** supplied conversation, plan/spec path, or issue identifier/URL; required to begin. Ask for one if absent. Fetch referenced bodies and all comments; report inaccessible material rather than inventing it.
- **Scope and decisions:** intended behaviors, constraints, acceptance/verification work and recorded owner decisions. Distinguish source/spec approval from breakdown/edge approval; neither implies the other. Draft unsettled choices as questions, not decisions.
- **Existing backlog:** associated children/tickets, full discussions and actual blocking relationships. Inspect before proposing new tickets. If inaccessible, mark coverage unverified and request access; provisional drafting may continue, but duplication-safe publication cannot.
- **Workspace/domain context:** supplied repository base and its guidance, glossary/ADRs when available. Exploration is optional if already sufficient or no repo is needed. Missing optional domain docs do not require setup or scaffolding.
- **Delivery mode:** proposal only, local publication or remote publication, with destination and write authority. Default to proposal only when unspecified; do not assume a local directory or configure a tracker merely to draft.
- **Publication conventions:** read existing project guidance and its tracker, domain and label pointers (commonly `docs/agents/{issue-tracker,domain,triage-labels}.md`), or use explicit owner instructions. Resolve conflicts with the owner. Missing destination/workflow details block affected writes, not drafting. Missing readiness-role mapping blocks creation or an authorized readiness/state change that requires that role, not unchanged reuse or scoped maintenance preserving labels/status. Offer `/setup-matt-pocock-skills` only if the owner wants configuration; never run it automatically.
- **Required context pointers:** exact source paths/URLs, sections, read timing and relative-path base. Missing material locations or approvals are inputs to obtain. Unknown owners/dates may remain “Not specified”; they do not block useful drafting.

Owner-selected schemas, artifact-only returns and configured ticket interfaces replace the corresponding default output structures below. Carry their required coverage, approvals, context, acceptance checks and dependency semantics in the selected form; append no second default report. Format choice grants no publication or implementation authority.

## Procedure

### 1. Establish the current contract

Use `read_file`, `search_files` and the configured tracker's read workflow to read the complete source, comments, approval records, backlog and edges. Map every source behavior and verification requirement to existing coverage, stale wording or an uncovered gap. Prefer authorized alignment of existing tickets to a second decomposition, even if the approved spec was finalized after the tickets.

Use project domain terms; read applicable glossary and ADRs before optional exploration. Surface ADR conflicts for decision, not silent override. Identify justified prefactoring: “Make the change easy, then make the easy change.” Do not invent implementation paths from an unexplored codebase.

**Done when:** coverage, recorded decisions, missing material inputs, proposed create/reuse/update actions and their authority are explicit.

### 2. Design slices and edges

Each normal slice is a narrow **complete end-to-end behavior** through only applicable layers (schema, API, UI, tests where relevant), independently demoable/verifiable and sized for a single fresh context window. Do not create a horizontal “all schemas” or “all UI” ticket. Put necessary prefactoring first and block only slices it genuinely gates; do not add gratuitous serial edges.

For a **wide mechanical refactor** that cannot land green vertically, use **expand–migrate–contract**: add the new form beside the old; migrate callers in blast-radius-sized batches (package/directory), each blocked by expand; remove the old form only after every migrate batch and proof that no caller remains. Keep CI green per batch when possible. If batches cannot be green alone, explicitly share an integration branch; all batches block a final integrate-and-verify ticket, and green is promised only there. Gate contract on the verified migration/integration result; never label a broken standalone batch independently green.

Record genuine prerequisite edges by stable existing IDs or draft numbers; ensure no cycles. Capture observable acceptance checks, relevant regression/negative cases and shared integration verification, not implementation recipes.

**Done when:** every requirement has a slice/check, every edge has a reason, and each exception has an honest verification boundary.

### 3. Present and obtain breakdown approval

Create **Ticket Proposal** using this template (or the owner's requested schema instead). When using the default format, use a numbered list including reused tickets and repeat its record for every proposal. In any selected schema, distinguish draft references from tracker IDs in its supported form and preserve the required semantics. Omit conditional parent work when not requested.

```markdown
# Ticket Proposal: [Name the supplied effort]
**Source / approval:** [Exact source and sections; recorded spec approval or “Not recorded”]
**Required context:** [Exact paths/URLs, read timing, path base; label local-only sources]
**Coverage:** [Map behaviors/checks to existing IDs or gaps; explain reuse versus creation]
**Publication:** [Proposal only, or intended configured destination and authority; not yet published]
1. **Title:** [Short domain-vocabulary title; existing ID if reused]
   **Action:** [Create, reuse unchanged, or proposed alignment; describe changed wording/edges]
   **Blocked by:** [IDs/draft numbers and genuine gating reasons, or “None (can start immediately)”]
   **What it delivers:** [End-to-end observable result; wide-refactor exception if applicable]
   **Acceptance criteria:** [Concrete behavior and verification checks, including integration boundary]
   **Context:** [Required source sections for this ticket; may reference common context above]
**Open inputs:** [Specific missing decisions/locations/access, or “None”]
**Approval requested:** Is the granularity right? Are only genuine blockers linked? Merge or split anything?
**Parent changes:** [Only if requested: exact proposed change and separate authorization needed]
```

Wait for an actual human decision on this breakdown and its edges; iterate and seek fresh approval after material changes. Prior spec approval, agent review, silence or labels are not breakdown approval. Retain recorded approval of an unchanged existing breakdown; do not force reapproval solely to reuse it. Changed requirements/edges need approval and write authority. Proposal-only requests end with the proposal and open inputs; approval is not required to deliver a draft.

**Done when:** the requested draft is delivered, or a specific breakdown/edge revision is human-approved and applicable publication inputs are settled. Stop before writes otherwise.

### 4. Publish only within authorized scope

Check the latest records for concurrent changes before any write. Use the configured issue-management workflow for approved reconciliation; do not run a blanket triage/closure workflow on these agent-ready tickets. For new tickets or expressly authorized readiness/state changes, resolve the configured `ready-for-agent` role string unless instructed otherwise; if no mapping exists, obtain confirmation of the default rather than inventing labels. Retained-ticket maintenance with no authorized label/status change preserves those values without requiring a new role mapping. Preserve unrelated labels, discussion and statuses.

**Local:** only when local publication is configured/selected and authorized, retain its existing default `.scratch/<feature-slug>/issues/<NN>-<slug>.md` relative to the explicit project base, unless the owner/config chooses another home. One new file per ticket, numbered from `01` blockers-first for a new backlog; preserve existing IDs/files and avoid collisions when extending it. Create directories only for this authorized publication. Use `write_file` for new files and `patch` for scoped existing changes; never overwrite discussions.

Create **one Local Ticket file per ticket**. Use this default template only when no owner/configured schema replaces it; preserve the same required semantics in the selected form:

```markdown
# [NN]: [Short domain title]
**Context:** [Exact source path/URL and sections; when to read; explicit base; local-only marker]
**What to build:** [End-to-end behavior, or named wide-refactor phase and boundary]
**Blocked by:** [Numbers/titles of actual prerequisites, or “None (can start immediately)”]
**Status:** [Configured ready-for-agent string for new tickets; preserve existing state unless authorized]
## Acceptance criteria
- [ ] [Observable success condition and how to verify it]
- [ ] [Relevant regression, negative case or shared integration check]
```

**Remote:** create one issue per new ticket blockers-first, retaining existing IDs for reuse. Set its title to the domain title and apply the configured ready label to new tickets. Link native blocking/sub-issue relationships where supported and applicable, with actual IDs; otherwise use explicit blocking references in the body. A child relationship is not itself a blocking edge. Do not create a parent just to fill a template.

Create **Remote Ticket body** using this default template only when no owner/configured schema replaces it. Preserve its required semantics in the selected form; omit `Parent` if the source was not an existing issue. State labels/native relationships in the write payload, not as fictional completed body fields.

```markdown
## Parent
[Actual source issue reference, only when applicable]
## Required context
[Exact source URLs/paths, sections and read timing; base for relative paths]
[Local-only inputs clearly labeled with owner-approved access/delivery route if necessary]
## What to build
[End-to-end behavior, or named wide-refactor phase and verification boundary]
## Acceptance criteria
- [ ] [Observable success condition and how to verify it]
- [ ] [Relevant regression, negative case or shared integration check]
## Blocked by
- [Actual blocking issue references, or “None (can start immediately)”]
```

Verify existing source targets/headings, current revision and read timing. Never substitute “the approved architecture” for a concrete pointer or present a local file as a published URL. If shared readers need unpublished material, obtain an approved accessible route or disclose the local-only boundary; do not upload/push just to create a link. Avoid speculative implementation files/snippets. When exact paths are explicitly requested, list only confirmed resource homes/interfaces, distinguishing existing inputs from resources to create and giving bases. A decision-rich prototype snippet may be inlined with prototype provenance; trim working-demo scaffolding.

Publication alone does not authorize changing parent guidance, extending its execution map or closing it. Parent-map extension/backlog alignment requires separate owner authorization, must preserve history and is not implementation completion; do not close the parent as a result of ticket creation. Stop at publication—do not implement or claim frontier tickets. Later execution may work any ticket whose blockers are done, not merely the next number.

**Done when:** approved records/edges/labels exist at the authorized destinations, or partial writes and remaining work are explicitly reported without blind duplicate retries.

### 5. Verify and deliver

Read back each exact published file/issue, full body and relationships; compare counts, IDs, acceptance checks, labels and edges with the approved proposal. Verify reused/updated records too; successful writes alone are insufficient. Report local paths with their base or actual remote URLs, reused/changed/created IDs, approval scope, unresolved inputs and any partial publication. Do not imply implementation, parent closure or remote accessibility that was not established. Owner-supplied schemas/artifact-only returns replace, rather than supplement, a default report.

**Done when:** the reader can reach all requested tickets and necessary context, and draft versus published versus unverified state is honest.

## Complete illustrative input/output

**Input (illustrative, not a live tracker exercise):** “Draft only from this conversation. Plan: a signed-in member can export their filtered purchases as CSV, excluding another member's records. The filter already exists and needs no prefactoring. Service and browser are applicable; no schema change. Priya approved the spec, not the breakdown. No associated backlog exists (owner confirmed). No repo or tracker supplied.”

**Complete output:**

```markdown
# Ticket Proposal: Filtered purchase export
**Source / approval:** Supplied conversation, Plan sentences; spec approved by Priya, breakdown not approved.
**Required context:** Read the supplied Plan sentences before implementation; no relative file paths.
**Coverage:** Owner confirmed no associated backlog. Ticket 1 covers the browser-to-service export, member isolation and end-to-end verification.
**Publication:** Proposal only; no tracker or local directory selected, no writes authorized.
1. **Title:** Download filtered purchases as CSV
   **Action:** Create; no existing ticket to align.
   **Blocked by:** None (can start immediately).
   **What it delivers:** A signed-in member downloads their filtered purchases as CSV from the browser; the slice includes its service behavior and needs no schema change or prefactoring.
   **Acceptance criteria:** Demonstrate browser-to-service download with CSV rows matching the member's filter; verify another member's records never appear; preserve the existing filter behavior.
   **Context:** Supplied Plan sentences: filtered CSV export and member isolation; read these before implementation.
**Open inputs:** None for drafting. Tracker/destination, labels and write authority are needed only if publication is requested.
**Approval requested:** Is the granularity right? Are only genuine blockers linked? Merge or split anything?
```

## Pitfalls

- Spec approval is not breakdown approval; an approved proposal is not automatic write permission.
- “Vertical” means applicable layers, not mandatory schema/API/UI work; wide refactors need their explicit exception.
- Existing implementation children may already be authoritative; do not clone them or renumber them to fit a new proposal.
- Draft numbers, parent/child links and blocking edges have different meanings; never invent real tracker identifiers.
- Configuration, implementation, claiming and closure are separate operations, not hidden publication prerequisites or side effects.

## Result checks

- All source behaviors and verification work are covered once; reused work and unresolved gaps are visible.
- Each slice fits a fresh context, is independently verifiable (or declares shared integration), and has only genuine acyclic blockers.
- Prefactor order, expand–migrate–contract edges and “green only at integration” promises are accurate where applicable.
- Source and breakdown approvals are distinct; changed breakdown/edges have actual human approval before publication.
- Templates contain concrete checks and current readable context pointers, not invented paths, approvals or URLs.
- Drafts require no setup, local home or write approval; affected writes use authorized homes and applicable readiness mappings, preserving retained labels/status when no change is authorized.
- Owner-selected schemas/artifact-only returns replace default headings, numbering and reports while retaining required semantics; no second deliverable is appended.
- Parent changes have separate authority; no implementation or closure is inferred from publication; all delivered routes/readbacks are accounted for.
