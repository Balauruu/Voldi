# Voldi — modular Phase 1 redesign

> **Status:** Working architecture brief. Requirements marked **confirmed** come from the user's answers; mechanisms marked **proposed** are not approved or implemented. Rethink the architecture completely, using the previous iteration for scope and principles—not as an implementation to patch. No production build or external writes are authorized in this session.

## Problem

Radu needs to understand a specific firm and choose a relevant business angle immediately before a cold call or first meeting. He has about two minutes to read. A generic industry description or product recommendation is not useful enough.

The [starting overview](voldi-architecture-overview.md) describes two historical pipelines with incompatible note formats, solution policies, verification, and destinations. It reports notes that passed structural tests but were generic or misleading in actual use. Those are documented findings, not independently reproduced results.

- **Current limitation:** The old architecture couples useful company research to competing workflows and template-driven conclusions. Rules, process history, and operational data are mixed.
- **Impact:** Passing format checks does not establish company-specific insight, a defensible angle, or usefulness during a sales conversation.
- **Context — confirmed:** Radu supplies selected firms from Pipedrive or LeadBridge. Most daily calls are quick qualification and need no full note; a normal research batch is 20–30 firms, not all approximately 100 daily calls.
- **Desired outcome — confirmed:** Short, scannable company preparation with exactly three Phase 1 modules: **company profile**, **SWOT analysis**, and **digitalization directions**. Later modules must plug in without changing existing modules.

### Evidence boundary

The previous iteration is supplied as documentation under [docs/prior-art/](docs/prior-art/README.md). It describes historical files and behavior; it is not the original executable project or evidence of current integrations. The two commercial digitalization PDFs and the two-page La Butic minute have been read through text extraction. Original code/tests, real exported company data, live source access, and CRM behavior have not been exercised here.

The prior-iteration documentation has now been read across the coordinator and a read-only worker. The coordinator also checked the load-bearing data/CRM handoffs, the documented SWOT intent, and verifier limitations against their source passages. This completes scope familiarization, not an audit or execution of the original implementation.

## Solution

### Confirmed product direction

A modular, skills-centered research/preparation system with useful helpers. It receives Radu's selected firm(s), reuses existing information, collects missing company evidence, and produces the three Phase 1 analyses. It does not source or select prospects.

### Key Features

1. **Company profile — content confirmed:** What the firm sells/does, whom it serves, how it operates, and relevant size/locations. Exclude the person Radu is talking to from this module's profile. CRM contacts may still be reused as supporting input; that does not require displaying them in the profile. Exact depth, financial detail, and reading-view layout remain open.
2. **SWOT analysis — dependency and evidence rules confirmed:** Consume the company profile and industry-grouped problem-pattern library. Build Weaknesses/Threats from company-evidence-backed pattern matches. Every S/W/O/T item is labeled `fact` (with source) or `hypothesis` (with supporting observations and reasoning); no forced quadrant entries.
3. **Digitalization directions — strict dependency confirmed:** Derive from SWOT, with each direction referencing a specific supported W/T item. Stay problem-first and portfolio-independent. If neither a fact nor an evidence-grounded hypothesis supports a W/T, return no directions and 2–3 discovery questions. Direction count/ranking and reading layout remain open.
4. **Reusable research:** Reuse LeadBridge's ListaFirme information plus contacts and CRM notes in Pipedrive. Do not re-collect information already available. Provenance, age, conflicts, and refresh rules need an agreed policy.
5. **Two operating modes through one entry point — confirmed:** A single company on demand and a next-day batch. One user-facing preparation entry point coordinates the three separate internal modules; separate user-facing module calls are not included in Phase 1.
6. **Extensibility:** Phase 1 contains exactly three business modules. Later modules can be added without editing existing module implementations. Research acquisition, presentation, and validation may be shared technical helpers; they must not be disguised additional business modules.
7. **Scannable consumption:** The useful reading view fits approximately two minutes before a cold call or first meeting. How the three outputs are presented within that window remains open.
8. **Pattern learning — confirmed design scope:** Seed industry patterns as `unvalidated`; supplied call minutes create or reinforce patterns, while explicitly unconfirmed hypotheses lower confidence. Accept one new minute in one ingestion step. This is supporting knowledge maintenance, not a fourth business module or permission to build it now.
9. **Earned publication autonomy — confirmed:** For Radu's Excel lead batches, deliver research notes to Pipedrive. Each industry starts supervised; 20 consecutive approvals without edits earn autonomous posting for that industry. A Radu edit/flag on a posted note demotes that industry and resets its counter. Fact/hypothesis labels remain visible in every posted note. See **Operation and delivery** for state, assignment, and feedback contracts; design only.

### Proposed architectural separation — not yet approved

- **Shared preparation:** Resolve company identity, load existing records, and acquire missing evidence once for the firm. Record source attribution and coverage.
- **Three business modules:** Confirmed strict chain: **company profile → SWOT → directions**. Each owns its result; shared evidence remains available for source checks. The proposed contracts below describe inputs, references, and failure behavior.
- **Shared delivery/checking:** Compose an agreed reading view and check evidence/format boundaries without adding a fourth analysis module.
- **Extension seam:** Give modules a stable input/output boundary and an explicit way to join a workflow. A new module should consume existing documented inputs and produce its own result, not require old modules to know about it or rewrite one another's results. Concrete contract, storage, orchestration, and compatibility rules remain open.

Claude Code is the selected host. Supporting responsibilities and the minimal data shapes below are design proposals, not an implemented framework, database, or skill set.

### Scope and Boundaries

- **In scope now — confirmed:** Design Phase 1's company profile, SWOT analysis, and digitalization directions, including the workflow and extension boundary. Progressively fill this brief and explain it visually.
- **Later phases — explicitly deferred:** Additional modules such as finding relevant projects and explaining them. Their possible future existence informs modularity only; do not design or build their business behavior now.
- **Excluded — confirmed:** Finding/selecting firms; named products/solutions, technical implementation, or explaining a specific solution for a client; authoring post-meeting minutes and summaries. Functional business workflows are permitted without selecting technologies or vendors. Solutions are custom and defined later with the solutions team after a meeting is booked.
- **Explicit scope change:** Reading a supplied minute to maintain problem patterns is now in scope; it supersedes the earlier blanket exclusion of post-meeting material. Minute authoring/reporting remains a separate future agent's responsibility. No feedback implementation or live library mutation is authorized here.
- **Phase 1 module limit — confirmed:** Exactly the three named business modules; no radar, additional qualification module, project finder, or reporting agent in this phase.
- **Authorized changes in this session:** `draft.md` and explanatory `voldi-architecture.html`; read supplied references and prior-iteration documentation. This remains architecture work, not implementation of even the three approved modules.
- **Not authorized here:** Modify the original repositories, import/install the supplied skill collection, scrape real prospects, schedule production jobs, or write to external services.
- **Historical principles:** The user says the old principles are good. Their mapping into concrete new rules still needs confirmation; old prohibitions on databases/multi-agent arrangements and old implementation interfaces are not automatically preserved.
- **Safety boundary:** No bypassing access controls. The future CRM publication/approval policy is defined below; credentials, enforceable permissions, personal-data handling, and deployment restrictions still require specification before live operation. This session authorizes no CRM writes or runtime state changes.
- **Decision authority:** The user confirms consequential choices or explicitly delegates them. Confirmed scope is not acceptance of the proposed architecture.
- **Planning mode:** Local, unpublished draft; no tracker has been configured or authorized.

## Variables and References

### Confirmed inputs and operating requirements

| Item | Confirmed requirement | Still open |
|---|---|---|
| On-demand request | Pipedrive lead name/ID or CUI | Ambiguous name resolution, missing identifiers, input interaction |
| Batch request | Radu-supplied Excel batch is the confirmed target for earned-autonomy delivery; label selection/LeadBridge export remain previously agreed inputs | Excel columns/mapping, exact configured label, selection snapshot and duplicates; no previous input is silently removed |
| Label example | User gave `To research` as an example | Not an existing verified label or selected literal configuration |
| Existing information | Reuse LeadBridge ListaFirme data, Pipedrive contacts and CRM notes | Access adapters, field mapping, precedence, freshness |
| Normal batch | 20–30 firms | Start time, larger-batch policy, operating conditions |
| Batch deadline | Ready by 08:00 the next morning | Timezone, latest safe start, interruptions and partial-result behavior |
| On-demand wait | Up to five minutes | Coverage/deadline conflict, slow sources, partial-result policy |
| Reading window | About two minutes before cold call or first meeting | Note sections, distinct sales-moment views, detail access |
| Source coverage | Attempt every named source; record absent, blocked, or unmatched sources | Reuse/refresh policy and bounded access attempts; no assumption that empty data means no problems |
| Angle evidence | Strict SWOT chain; each direction links to an evidence-supported W/T, labeled fact or hypothesis | Ranking, contradictory-evidence policy; no supported W/T means no directions and 2–3 discovery questions |
| Operating system | Windows initially; cross-platform operation preferred | Portability acceptance and platform-specific access constraints |
| Runtime | Claude Code; skills-centered with useful helpers | Model, persistence, orchestration, integration details |
| Required Pipedrive MCP | `https://mcp.pipedrive.ai/mcp` — exact endpoint supplied by the user | Claude Code configuration scope, authorization, available tools, and verified read/write policy |
| Optional Pipedrive API — assessed below | Direct API use is permitted if useful | Documentary assessment complete: a bounded read-only helper is a credible complement, not yet selected. Compare the authenticated MCP path before choosing responsibilities |
| Note destination and approval | Pipedrive; supervised review by default, autonomy after 20 consecutive unchanged approvals per industry | Publication transport, review/edit detection, storage and permissions; see **Operation and delivery** |

**User-named research sources:** `listafirme.eu`, Google reviews, `undelucram.ro`, `glassdoor.ro`, and the firm's own website. Preserve `listafirme.eu` as given until its identity is resolved; historical docs mention a different ListaFirme hostname. Source methods and access availability remain unverified. The user's reuse requirement means coverage should not cause duplicate acquisition of already-held material; the exact status/refresh contract remains open.

### Reference roles

- **Historical scope/principles:** [voldi-architecture-overview.md](voldi-architecture-overview.md) and [prior-iteration documentation index](docs/prior-art/README.md). Use them for orientation and failure mechanisms, not as binding architecture or instructions to execute.
- **Business-area inspiration only:** [Digitalizare Retail.pdf](docs/Digitalizare%20Retail.pdf) and [Digitalizare Servicii Business.pdf](docs/Digitalizare%20Servicii%20Business.pdf). These suggest areas such as document handling, customer communication, stock/queue monitoring, flexible work, and information security. They are never a product catalog for Voldi's output. PDF product names/technical details must not migrate into the preparation note; generic advertised benefits do not prove a firm's needs.
- **Private calibration/feedback source only:** [Minuta_LaButic_Eduard.pdf](docs/Minuta_LaButic_Eduard.pdf), 13.10.2026, pp. 1–2. Page 1, “Ce își dorește,” reports manual AI-assisted onboarding and the requested invoice → specs/photos → descriptions → website → marketplaces workflow, with owner validation of price/photos only. This is the sole confirmed case, not evidence that other firms have this problem. The public signals named by the user are leads for a future public-only fixture, not public sources verified in this session. Description style suggesting AI is itself a hypothesis; approximate catalogue counts are qualified in the minute.
- **Reference age:** Business Services contains slides dated 22 September 2020; Retail metadata identifies a catalog dated 16.12.2021 and a PDF created in January 2022. Treat benefit claims and relevance critically; current commercial offer verification is outside product selection because Phase 1 does not select products.
- **Explanation artifact:** [voldi-architecture.html](voldi-architecture.html). Its mechanism choices are proposals; it must stay synchronized with settled requirements.
- **Required CRM connection — user-confirmed:** [Pipedrive MCP endpoint](https://mcp.pipedrive.ai/mcp). This connector must be set up for the selected Claude Code host. The exact supplied endpoint is preserved. Adding it to this brief does not configure or authenticate it; no live CRM tool inventory or read/write behavior is verified yet.
- **CRM access architecture — documentary assessment complete:** The comparison below covers lead selection, organization/CUI resolution, contacts, existing notes, pagination, batch reliability, and permissions against current official sources. MCP setup remains a confirmed prerequisite; API usage is a candidate complement, not a replacement already chosen. Historical connector limitations are not evidence about today's server. Publication policy is now defined under **Operation and delivery**; the read-only preparation adapter and publication boundary remain separate responsibilities.

### Prior-iteration orientation — documented facts, not adopted interfaces

- **Reusable evidence has pointers:** Historical LeadBridge manifests contain `cui`, `company_name`, `row_index`, `page_file`, and `url`. The workbook supplies a row and saved HTML supplies registry material. The new design should retain origin/provenance, but actual export compatibility remains unverified. See [data model, §6.1 batch manifest](docs/prior-art/06-data-model.md).
- **Lead identity is not company identity:** The docs place CUI on the linked Pipedrive Organization, with Lead links to Organization and Person. They explicitly say the old lot pipeline and CRM were not connected in code. New adapters must resolve these relationships rather than assume a CUI exists directly on every Lead. See [data model](docs/prior-art/06-data-model.md), opening diagram and §6.7.
- **Selection and research must be separate:** The old connector paginated all leads to filter a label, whereas the old REST helper returned the first matching item. These describe old adapters, not the new batch semantics or current API capability. See [Pipedrive integration](docs/prior-art/08-pipedrive.md), §§8.3 and 8.6.
- **SWOT has no old exercised implementation to preserve:** The historical dossier was specified but not built. It distinguished internal strengths/weaknesses from external opportunities/threats, while depending on inputs outside today's confirmed Phase 1. That distinction is a useful proposed definition; radar, call feedback, qualification grids, and catalogs are not newly required dependencies. See [hunting, dossier](docs/prior-art/04-hunting.md#dossier-phase-2-specified-not-built).
- **Quality needs meaning checks:** Both historical verifiers were documented as checking form, not usefulness; factual truth and identity were explicitly outside the hunting script's checks. Reusing the old pass/fail gates would not establish today's required quality. See [verification and tests](docs/prior-art/07-verification-and-tests.md), opening statement and §7.1.
- **Old timing is not a new guarantee:** A historical trial recorded about 7.5 minutes per company. It neither proves nor disproves that the redesigned path can satisfy five minutes. Timing and source-coverage failure behavior need explicit design and later exercise. See [hunting trial results](docs/prior-art/04-hunting.md#46-trial-results-recorded-in-plan-30-status).

**Recommended principle transfer — not blanket adoption:** Preserve fact/hypothesis/unknown distinctions, honest gaps, company-specific reasoning, provenance, and checks on usefulness. Replace the competing workflows and template/product-selection mechanisms. Do not import old freshness windows, scoring, output labels, file layout, or platform rules as new requirements.

## Workflow

### Confirmed workflow boundary

Radu selects the firms and requests preparation. Voldi reuses available data, runs the three Phase 1 analyses, and presents a short research note. For the Excel batch target, supervised notes require Radu's approval; earned-autonomy industries post directly to Pipedrive under the publication gate below. The business scope still ends at cold-call/first-meeting preparation, before technical solution definition or post-meeting report authoring.

### Proposed operating sequence — pending confirmation

1. **Resolve the request:** Interpret a lead name/ID, CUI, label selection, or LeadBridge export. Bind evidence to the right firm and CRM targets. Ambiguity should be exposed rather than silently merged; exact identity/group rules remain open.
2. **Reuse before acquiring:** Load existing LeadBridge registry material and Pipedrive contacts/notes. Distinguish source statements, user notes, and historical model output; do not assume every existing note is verified evidence. Decide whether any missing/stale fields require action under the still-open freshness policy.
3. **Cover the named sources:** Use existing material where applicable; seek missing material from every required source. Keep an honest coverage record, including absent, blocked, and unmatched outcomes. No bypassing access controls and no converting failure into a negative business finding.
4. **Run the strict chain:** Company profile → SWOT using matched industry patterns → directions linked to supported W/T items. Preserve fact/hypothesis labels and evidence references throughout. Without supported W/T, the final module returns no directions and 2–3 discovery questions; missing/failed upstream results never authorize bypassing SWOT.
5. **Check and deliver:** Check attribution, reasoning, specificity, and the no-products/technical-details boundary; preserve visible fact/hypothesis labels. Apply per-industry supervised/autonomous delivery to Pipedrive. Exact layout, storage, and enforcement mechanisms remain open; destination and promotion/demotion rules are now settled.
6. **Handle timing and failures:** Respect the five-minute on-demand expectation and 08:00 batch deadline under agreed conditions. Define explicit partial/failure behavior where full coverage or adequate evidence cannot be achieved; do not claim deadline compliance before the real path is exercised.

Single-company and batch modes should share the same module contracts and research semantics; batching changes how requests are supplied and progress is handled, not what counts as a company finding. This is a recommendation, not a selected scheduler or concurrency design.

### Module calls and updates — explanation proposal, not selected behavior

**Confirmed call pattern:** One public preparation entry point in Claude Code, coordinating all three separate internal modules. Modularity does not require three separate user-facing commands. Individual user-facing module calls are not selected for Phase 1.

- **Full preparation request:** Request all three analyses for the supplied firm(s). The proposed entry skill resolves the target, reuses/prepares evidence, and invokes the requested business skills. Claude Code is the selected host; exact skill names and request syntax remain proposals pending its documented skill mechanism.
- **Internal module rerun:** Recompute a module from evidence already held when needed within preparation. This is a reasoning update, not a separate user-facing Phase 1 command or permission to rescrape sources or silently change other results. Exact triggers remain open.
- **Update company information:** Acquire new/missing source material under an agreed reuse/freshness policy. This can affect more than one analysis; affected outputs must be recomputed or visibly marked outdated under a policy still to be chosen.

Avoid the ambiguous phrase "refresh a module" until we specify whether it means rerunning analysis or acquiring newer evidence. Dependencies are settled; freshness triggers and automatic-rerun policy remain open. A changed profile can invalidate SWOT and directions; changed SWOT can invalidate directions. Do not present stale downstream results as current.

### Operation and delivery

**Confirmed target:** Radu supplies an Excel batch of selected leads. The strict Profile → SWOT → Directions chain produces a research note per eligible lead; approved supervised notes and autonomous notes are posted to Pipedrive. Rejected, unresolved, or blocked rows retain an explicit outcome—not a false “posted” status. No application, writer, state store, or integration is built here.

```text
Excel row → CRM identity + industry assignment → research → note checks
  supervised industry → draft → Radu's review → approved note → Pipedrive
  autonomous industry → checked note → Pipedrive (no per-note approval)

Industry: supervised / counter 0
  20 consecutive approvals without edits → autonomous / counter 20
  Radu edits or flags any posted note → supervised / counter 0
```

Autonomy belongs to the **industry**, not a batch, lead, pattern, or the whole system. No credit transfers between industries; a newly encountered industry always starts supervised, even if other industries are autonomous.

#### Industry assignment — proposed policy, same taxonomy as patterns

Use one stable `industry_id` from a shared industry registry (label, business-activity definition, optional CAEN hints). `Pattern.industry` uses that same ID; the broad examples online retail, HoReCa, schools, and agriculture are candidate categories, not a finalized taxonomy.

1. Prefer Radu's explicit Excel industry value, mapped to a canonical ID; do not use arbitrary spelling or a batch name as a new trust bucket.
2. If absent, assign the primary operating industry only when the resolved company's profile and attributable business activity support one unambiguous category. Registry CAEN is a supporting hint, not sufficient alone where actual activity differs. Record assignment basis and source references.
3. Conflicting, ambiguous, or mixed-activity assignments stay `unresolved` until Radu settles the primary category. Such notes remain drafts, cannot auto-post, and earn no promotion credit; never borrow the most permissive industry's mode. Supporting industry tags may aid research but do not control the gate.
4. Bind review/publication to the assignment revision. Reclassification requires explicit reassessment; do not transfer a streak or silently reinterpret historical feedback. An unknown category has no earnable shared “unknown” counter.

Excel-to-lead matching must resolve exactly one Pipedrive lead before posting, using a supplied valid lead ID where available or the existing identity-resolution rules. Workbook columns and mapping remain open; no import/create-lead permission is implied.

#### Per-industry state and transitions

**Minimal proposed records** (logical shapes only):

```text
IndustryState {
  crm_account_ref, industry_id,
  mode: supervised|autonomous,
  consecutive_approval_counter: integer 0..20,
  state_revision, last_event_ref
}
NoteDelivery {
  note_id, note_revision, research_run_ref,
  batch_ref, row_ref, industry_sequence, crm_lead_id: optional,
  industry_assignment: {industry_id: optional, revision, status: assigned|unresolved, basis, source_refs},
  status: draft|awaiting_review|approved|rejected|posted|blocked,
  industry_state_revision: optional, crm_note_id: optional, publication_key: optional
}
NoteFeedback {
  feedback_id, sequence, actor, note_ref, industry_assignment_ref,
  action: approved_without_edits|approved_after_edits|edited|rejected|flagged,
  before_revision, after_revision: optional, reason: optional,
  pattern_refs: [pattern_id + revision], minute_ref: optional
}
```

State is scoped to the configured CRM account and canonical industry. Only Radu's review of a tracked workflow note earns approval credit. Feedback binds the exact note/industry revision; edits retain before/after content or a located diff, and rejections/flags retain the affected note and any supplied reason. Missing reasons never prevent logging. `approved_after_edits` permits supervised delivery, not streak credit. `industry_sequence` records generation order; feedback `sequence` records receipt order. Publication key/state/target fields are mandatory before posting, but may be absent on unresolved drafts.

| Event | Mode/counter effect | Delivery effect |
|---|---|---|
| First encounter with industry | `supervised`, `0` | Draft; Radu reviews every note. |
| Supervised original note approved by Radu without edits | Increment by one; reaching **20** promotes automatically | Post that approved revision. The twentieth note still required review; subsequent eligible notes need none. |
| Supervised note edited or rejected | Stay supervised; reset to `0`; log every edit/rejection | Reject: do not post. Edit: only Radu's approved final revision may post; it never counts as an unchanged approval of the original note. |
| Autonomous eligible note | Stay autonomous; counter remains `20` | Post directly after checks, without per-note human approval. Autonomous posts do not create new approval credit. |
| Radu edits or flags a posted note | Owning industry becomes `supervised`, counter `0`, even for a historical posted note | Stop further autonomous publication for that industry; queued notes return to supervised review. Other industries are unchanged. |

**Proposed ordering/replay discipline:** Count once per original generated note, in its recorded generation order within the industry—not by selecting the easiest reviews first. An earlier unresolved review blocks promotion credit from later approvals. Edits/rejections break the streak; an edited note's later approval does not erase that break. Replayed feedback and multiple approvals of revisions of the same note cannot add credit. Demotion invalidates old earned credit; historical approvals are not replayed to re-promote. Serialize state transitions and recheck the current state immediately before publication; reconcile already-in-flight requests rather than claiming they can be recalled.

#### Publication gate and visible evidence

Earned autonomy removes **manual approval**, not the existing identity, strict-chain, evidence, or quality checks. Block posting when the CRM target/industry is unresolved, upstream output is missing, checks fail, or the current state cannot be established. A valid `discovery_only` note may post with its 2–3 questions; never fabricate directions to fill it.

Every posted note keeps readable `fact` / `hypothesis` labels beside its claims, source references for facts, and the referenced W/T status beside each direction's motivating problem. A proposed direction stays a proposal. Renderer/CRM formatting must not strip these distinctions; approval does not convert a hypothesis into a fact.

**Proposed narrow write boundary:** Publication creates research notes only; the CRM preparation helper remains read-only. No autonomous edits/deletes of existing notes or changes to lead fields/labels are implied. Bind `publication_key` to account + lead + run + note revision, record the returned CRM note ID, and read back the exact target before marking `posted`. An ambiguous write result requires reconciliation, not a blind retry that creates duplicates. Radu's own CRM edits are feedback inputs, not permission for Voldi to rewrite CRM history.

#### Fit with pattern/minute learning

- `NoteFeedback` records **note-quality decisions** and controls industry autonomy. `MinuteIngestion.assessments` records **business-need confirmation** and controls pattern confidence. These are separate evidence streams: an unchanged approval is not a confirmed need; a stylistic edit/rejection is not `not_confirmed` evidence about the business problem.
- Add optional `industry_assignment_ref` and `note_feedback_refs` to `MinuteIngestion` so a single supplied minute can link to its prior note/review. `Pattern.feedback_refs` keeps its existing minute-assessment meaning; quality events may reference affected patterns without directly increasing/decreasing case-confirmation counts.
- If a correction includes located evidence confirming/refuting a need, link that evidence through the minute-assessment contract rather than infer it from the review action. A minute alone never increments the 20-note counter or grants posting authority.
- Review/edit capture, especially edits made directly in Pipedrive, must be defined and exercised before autonomous operation. If that feedback path is unavailable or unverified, autonomous posting is held; earned state is not fabricated or silently reset. Model/library-change requalification and final industry definitions remain open policies.

#### Contradictions and design checks

**Explicitly superseded:** Earlier statements that output destination, approvals, and automatic CRM writing were unselected. They are now selected for this target: Pipedrive publication with the specified industry gate. The earlier “no automatic CRM writer” recommendation is superseded only for this narrow future publisher, not for a general CRM integration. Preparation reads remain separate. **Unchanged:** no implementation, authentication, or external writes are authorized in this design session.

**Input-scope change:** Excel is now the confirmed batch target; previously agreed on-demand/label/export inputs are not silently deleted. **Deadline tension, unresolved:** the existing 08:00 readiness requirement cannot guarantee supervised posting without Radu's approval. Decide whether that deadline means drafts ready or notes posted; do not mark review-pending notes as delivered.

**Acceptance checks to exercise later, NOT RUN:** For one industry, 19 unchanged approvals leave it supervised; approval 20 promotes it and the next eligible note posts without review. An intervening edit/rejection resets the streak. A new industry starts at zero. A posted-note edit/flag demotes only its recorded industry and blocks queued autonomous writes. Unresolved assignments, replayed events, and edited revisions cannot earn credit. Every CRM note readback retains the visible labels; note review and minute learning affect only their own counters/confidence. These checks do not establish that 20 approvals guarantee future factual correctness.

### Module contracts — confirmed chain, proposed minimal shapes

```text
Company evidence → Profile → SWOT → Directions or discovery questions
                              ↑
                   Industry problem patterns
Supplied minute → feedback evidence → future pattern-library revision
```

Arrows denote required analysis inputs or knowledge updates, not implemented calls. Feedback never rewrites a completed run; each run records its input/library revisions.

| Module | Required inputs | Owned output and checks |
|---|---|---|
| **Company profile** | Resolved company identity; evidence with source/locator, observation/retrieval dates, coverage and conflicts | `profile_id`, `revision`, business activities/customers/operations/size/locations, attributed observations and gaps. Exclude the contact person. Missing facts stay unknown; no invented current process. |
| **SWOT** | Profile revision; its evidence; applicable industry pattern-library revision | `swot_id`, `revision`, `profile_ref`, `library_revision`, `items[]`. Each item: `item_id`, `quadrant: S|W|O|T`, `claim`, `label: fact|hypothesis`, `evidence_refs`, and hypothesis `reasoning`; W/T also carry `pattern_ref` and matched signal references. Distinguish internal W from external T. Pattern confidence is not proof of this firm's problem. |
| **Directions** | SWOT revision and its referenced profile/evidence | `swot_ref`, `status: directions|discovery_only|blocked`, `directions[]`, `discovery_questions[]`. Each direction: `direction_id`, one specific `swot_item_ref` (W/T), problem-first business change, why it addresses that item, inherited `problem_label: fact|hypothesis`, evidence references, and a question testing the need/current process. The business change is a proposal, never a fact merely because its referenced problem is factual. No fixed portfolio, products, invented savings, or unsupported workflow detail. |

**Gate:** A supported W/T may be a sourced fact or a hypothesis grounded in company observations and an explained pattern match; generic industry membership alone is insufficient. No supported W/T ⇒ `discovery_only`, empty directions, exactly 2–3 non-leading questions. Failed/missing profile or SWOT ⇒ `blocked`, name the missing prerequisite; do not invent analysis. S/O need not come from problem patterns. Rules for a novel evidenced W/T without a library match remain open: do not force it into an unrelated pattern.

### Problem-pattern library and single-minute feedback — design only

**Pattern meaning:** observable signal(s) → likely internal weakness/external threat → problem-first digitalization direction → discovery question. Group by industry. Patterns guide hypotheses; matched company observations and counter-evidence decide whether a W/T is supportable, not canned industry text.

**Minimal proposed records** (type notation, not persisted data). IDs are stable; module/pattern references bind ID plus revision; source references carry a resolvable location and passage/page. `public_signal_refs` refer to public evidence, never private-minute passages relabeled public. Required record fields are shown; only fields marked optional may be absent.

```text
Pattern {
  pattern_id, revision, industry,
  signals: [{signal_id, observable, role: required|supporting}],
  counter_signals: [observable],
  problem: {quadrant: W|T, claim},
  direction, discovery_question,
  feedback_refs: [minute_id + assessment_id],
  confidence: derived {status, confirmed_case_count, not_confirmed_case_count}
}

MinuteIngestion {
  minute_id, revision, company_ref, meeting_date,
  source: {path, locator}, research_run_ref: optional,
  industry_assignment_ref: optional, note_feedback_refs: optional [feedback_id],
  assessments: [{
    assessment_id, hypothesis_ref: optional, pattern_ref: optional,
    outcome: confirmed|not_confirmed|not_assessed,
    need, evidence_locator,
    public_signal_refs: [source + locator],
    proposed_pattern: optional Pattern
  }]
}
```

- **Matching:** All required signals must be backed by the firm's evidence; supporting signals strengthen but never replace them. Record counter-signals and reject contradicted matches. A statement that descriptions “look AI-generated” cannot become factual proof of AI use or manual onboarding.
- **Confidence:** Derive `status: unvalidated|case_supported|contested` and distinct-company confirmation/nonconfirmation counts from referenced assessments, not a made-up probability. Zero assessed cases = `unvalidated`; confirmation adds support; explicit nonconfirmation lowers support and marks conflicting evidence as `contested`. Preserve both sides. `not_assessed` means not discussed/unclear, not a negative result. One La Butic confirmation establishes case support only, not industry-wide validation; no numeric threshold is selected.
- **One-step intake:** Submit one source minute; the proposed maintenance helper resolves the firm, extracts located assessments, links prior hypotheses/patterns or proposes a new pattern, and records the minute plus resulting library revision in one operation. No manual batch assembly required. `(minute_id, revision, assessment_id)` prevents replay from double-counting; corrections supersede earlier assessments without erasing their provenance. One company does not become several independent confirmations through repeat minutes.
- **New pattern:** A confirmed need without a matching pattern can create a case-supported candidate, with signals only where public-source provenance exists. If no observable trigger is known, keep it unmatchable pending that evidence; do not invent one from private call detail. Storage, identity matching, extraction review/approval, and numeric scoring remain open.

**Proposed seed examples — unvalidated generic priors, not observed cases:**

| Industry | Observable signal combination | Likely W/T hypothesis | Direction → discovery question |
|---|---|---|---|
| Online retail | Frequent new product listings; sizeable catalogue distributed through multiple channels | W: manual product enrichment/onboarding may constrain catalogue updates | Streamline product-data preparation and publication → “How does a new supplier product become a complete listing on your site and marketplaces?” |
| HoReCa | Bookings offered through several public contact channels without a visible unified booking route | W: fragmented booking coordination may cause re-entry or missed requests | Centralize request/booking handling → “How do you reconcile bookings received by phone, messages, and your site?” |
| Schools | Public enrolment instructions requiring repeated document submission across separate steps | W: manual enrolment/document reconciliation may burden staff | Streamline enrolment information and document handling → “How is enrolment information handled; is anything re-entered or reconciled manually?” |
| Agriculture | Several advertised storage/operating locations and explicit traceability/reporting obligations | W: cross-location records may be fragmented; T only if a relevant external obligation is sourced | Consolidate operational records and traceability → “How do you assemble a lot's records across locations for reporting?” |

These are proposed signals to investigate, not evidence of a real firm's inefficiency. La Butic alone may raise the retail pattern's case support after located feedback is recorded; the other seeds remain unvalidated. No library or ingestion records have been created.

### La Butic acceptance spec — defined, NOT RUN

**Purpose:** Check whether public evidence can yield the correct problem hypothesis, SWOT-linked direction, and discovery question. It is a calibration regression, not proof of generalization from one known case.

**Fixtures to prepare later:** Correctly identified `LA BUTIC ON-LINE S.R.L.` / `labutic.ro`; a dated, source-located public-only evidence packet; frozen generic industry seed-library revision; separately sealed [internal minute](docs/Minuta_LaButic_Eduard.pdf), p. 1, “Ce își dorește,” as the evaluation oracle. The user's public-signal leads are shop activity since 2011, approximately 1,200 products, frequent new listings, eMAG/EasySales selling, AI-like descriptions, and registered own brands. Verify each against genuinely public sources; do not extract them from the minute and relabel them public. Neither brand protection nor shop age alone proves onboarding pain.

**Procedure, only after separate execution authorization:**
1. Freeze public inputs and library revision. The research executor receives generic module rules, public evidence, and generic seeds only—not this case's expected answer, the minute, private CRM notes, conversation, or La Butic confirmations/company-specific pattern rules. Generic seed wording is still a known-case design prior, so this remains calibration, not a blind holdout. Hold out its minute-derived feedback until after output is frozen. If public identity/source coverage cannot be established, report `blocked/inconclusive`, not pass.
2. Run Profile → SWOT → Directions once and freeze all outputs before opening the oracle. Retain evidence references, pattern matches, revisions, and an input-access manifest. No target-specific tuning between generation and scoring.
3. Radu compares the frozen output with the minute and records pass/fail for every criterion below, with output item IDs and oracle locators.

**Pass requires all:**
- [ ] SWOT contains a **Weakness equivalent to manual product onboarding/enrichment/publication**, not merely “needs digitalization.” Public evidence supports the reasoning; absent direct public proof, it is labeled `hypothesis`, never `fact` inferred from the minute.
- [ ] A problem-first direction references that exact W item and addresses reducing manual collection/enrichment of product data and publication/distribution to the shop/marketplaces; it is not an unrelated security/ERP/product recommendation.
- [ ] A non-leading discovery question tests who currently obtains specs/photos, prepares descriptions, publishes listings, and whether/which steps are manual. Functional equivalence is judged by Radu, not exact wording.
- [ ] Every SWOT item has the required label; facts have source references, hypotheses have supporting observations/reasoning; every direction links to a supported W/T. No private-case leakage or fixed portfolio restriction.

**Oracle fidelity:** The minute confirms supplier invoices as the starting data, automatic specs/photo search, AI descriptions, site upload then marketplace distribution, and price/photo-only owner validation (p. 1). Those private details are comparison context, not mandatory pre-call discoveries or facts the public-only output may invent. Supplier invoice format and synchronization details are still questions in the minute (p. 2).

**Companion failure check, also NOT RUN:** With a fixture having no evidence-supported W/T, require no directions and 2–3 discovery questions. A missing upstream result must instead return `blocked`. Retest feedback separately: one confirmed minute raises case support; an explicit nonconfirmation lowers it; unassessed items and replay do not change counts. Passing La Butic does not validate the other industry seeds or meet whole-product timing acceptance.

### Contradictions and limitations explicitly recorded

- **Superseded by the user's decision:** Dependencies previously marked open, and the assistant's proposed profile fan-out/optional SWOT input. The strict chain now governs the brief and diagram.
- **Scope conflict, explicitly amended:** The old exclusion of all post-meeting material conflicts with minute-based learning. Supplied-minute ingestion is included only as supporting pattern maintenance; minute/report authoring remains excluded and exactly three business modules remain.
- **Functional versus technical detail:** The minute contains named systems and implementation questions. Use its confirmed business need for calibration; do not turn existing systems or the PDFs into a fixed solutions portfolio. Directions may describe business workflow changes, not vendor/technical implementation choices.
- **Historical failure risk:** Pattern matching must not recreate template-only industry notes: each proposed W/T requires actual matched observations, reasoning, and counter-evidence handling. Confidence from one case never substitutes for company evidence.
- **Test limitation:** Public-only inputs can still overfit a known calibration case. Hold out its feedback and private material; true blind generalization requires a different future case. Public fixtures and the acceptance execution remain pending.

### Concrete Claude Code shape — proposed, not created

Claude Code supports project skills at `.claude/skills/<name>/SKILL.md`, direct `/skill-name` invocation, and passing trailing input through `$ARGUMENTS`.[1] Proposed Phase 1 skill layout, relative to the future Voldi project root (not files created in this planning workspace):

```text
.claude/skills/
  voldi/SKILL.md                     # public preparation entry; coordinates only
  company-profile/SKILL.md           # internal business module 1
  swot-analysis/SKILL.md              # internal business module 2
  digitalization-directions/SKILL.md  # internal business module 3
```

Proposed interaction: `/voldi <lead ID, CUI, lead name, label request, or export path>`. This is one skill with different request inputs, not five independent commands or an installed parser. Request syntax/validation remains to be specified.

Claude Code documents `disable-model-invocation: true` for manual-only skill invocation and `user-invocable: false` for skills invoked only by Claude, hidden from the user's command menu.[1] Proposed mapping: an explicitly invoked `voldi` entry, with the three internal skills not exposed as separate user commands. These controls govern skill invocation; they are not a data-access sandbox or a guarantee that workflow dependencies are enforced. Permission and module-contract checks need their own design.

Claude Code documents HTTP transport for remote MCP servers and an explicit `type: "http"` plus URL for JSON server configuration.[2] Proposed project configuration, shown here only—not written to `.mcp.json` or authenticated:

```json
{
  "mcpServers": {
    "pipedrive": {
      "type": "http",
      "url": "https://mcp.pipedrive.ai/mcp"
    }
  }
}
```

Project-versus-user configuration scope, account authorization, live tool inventory, and permissions remain setup decisions. The completed MCP/API assessment below is documentary research; it must not be mistaken for a working connection or an approved direct-API client.

### CRM access assessment — findings and proposed responsibility boundary

**Conclusion:** Direct API access is useful as an option for deterministic CRM preparation, especially complete batch selection, account-specific CUI mapping, and note/context joins. It is not necessary for the business reasoning itself. **Recommendation, not owner adoption:** start with the required official MCP connection; add a small read-only API helper only for a demonstrated capability, completeness, or operational-control benefit. If MCP meets the same contract, avoid maintaining a second implementation.

#### What is established, and what is not

Pipedrive's published MCP catalog includes `getLeads`, `getLead`, `searchLeads`, organization/person reads and searches, and `getNotes`/`getNote`.[3] It also includes create/update operations, including `addNote` and `updateNote`; it is not a read-only connector by default.[3] The catalog lists tool names and purposes, not their input/output schemas or continuation contracts. No label-list, saved-filter-list, or field-metadata tools appear in that inspected catalog; absence there does **not** prove absence from the authenticated server.[3]

The official Claude connection guide confirms the exact supplied endpoint and says access inherits the user's Pipedrive role and visibility settings.[4] Its connector UI instructions describe Claude, not an exercised Claude Code setup. Claude Code's own HTTP/OAuth mechanism supplies the proposed host integration.[2] Public documentation does not establish that the Pipedrive MCP server accepts a reduced read-only OAuth grant; inspect actual consent and capabilities before relying on that boundary.

| Required operation | Documented MCP route | Documented REST alternative and its implication |
|---|---|---|
| Lead by supplied ID or name | `getLead` / `searchLeads`; live accepted parameters and returned links unverified.[3] | Exact retrieval uses `GET /api/v1/leads/{id}`; keyword search uses `GET /api/v2/leads/search`.[5][6] A search result is a candidate, not authority to select the first similarly named firm. |
| Complete selected-label batch | `getLeads` exists; label/filter inputs, archive handling, and pagination remain unverified.[3] | `GET /api/v1/leadLabels` resolves actual label UUIDs. `GET /api/v1/leads` returns nonarchived leads with `start`/`limit` and `filter_id`, but **no documented direct label-selection query parameter**.[5] Inspect a supplied existing filter, or enumerate all visible nonarchived leads and match exact returned `label_ids`; do not create filters during this read-only preparation. |
| Organization and CUI | `getOrganization`, `getOrganizations`, and the singular `searchOrganization` are published; custom-field inclusion/search is unverified.[3] | `GET /api/v2/organizationFields` exposes `field_name`, `field_code`, and `field_type`; organizations expose custom-field values.[6] Confirm the account's CUI field rather than assume a built-in `cui` property or inherit old hash keys. |
| CUI-only request | Published organization search, without a documented field-specific MCP contract.[3] | `GET /api/v2/itemSearch/field` supports a chosen organization field with `match=exact`, subject to its documented searchable field types; matching is case-sensitive.[6] Fetch candidates and check the actual CUI value; duplicate/malformed/missing identifiers remain explicit cases. |
| Reusable contacts | `getPerson`, `getPersons`, `searchPersons`; complete contact data and relation filters unverified.[3] | V2 organization/person list operations accept batches of up to 100 IDs; persons also support `org_id`.[6] Follow explicit lead links first. Missing organization links or conflicting person/organization links must not be silently repaired. |
| Existing CRM notes | `getNotes` / `getNote`; entity filtering, full content, and paging unverified.[3] | `GET /api/v1/notes` supports separate lead, organization, and person attachment filters plus offset pagination.[5] Union the relevant collections and deduplicate by note ID, retaining attachments/timestamps. Person-record `notes` is contact-sync dependent, not the full Notes collection.[6] |

REST is not uniformly v2: the inspected lead-list, label, filter, and note operations remain v1, while the relevant organization/person/field/search operations above use v2.[5][6] Documentation living under `/docs/api/v1/` does not make every documented operation v1. Lead/lead-label IDs are UUIDs, while organization/person IDs are integers in the inspected schemas.[5][6]

#### Two response-contract traps to verify before implementing

- **Lead pagination:** the general offset guide describes `additional_data.pagination` with `next_start`.[7] The inspected lead-list OpenAPI instead places `start`, `limit`, and `more_items_in_collection` directly under `additional_data`, omits `next_start`, and omits metadata from its example.[5] Do not copy a notes paginator or guess the live shape. A future adapter must check advancing pages, repeated results, and terminal metadata; unexplained missing metadata is a visible incomplete selection.
- **Field-search decoding:** its current v2 schema describes entries with `result_score`/`item`, while the example shows direct `{id,name}` entries.[6] This inconsistency needs an actual permitted response before fixing the decoder. It is not proof that field search is broken.

#### Smallest useful helper — proposal, not a fourth module

```text
/voldi request
  shared CRM preparation
    official MCP reads where verified sufficient
    optional REST helper for specifically assigned gaps/control benefits
  one normalized selection manifest + company evidence packet
  company profile → SWOT (industry patterns) → digitalization directions
```

Indentation shows proposed CRM responsibility; the business-module chain is now confirmed separately. The CRM routes must not acquire the same records twice by default or create two competing research pipelines.

- **Inputs:** Tagged lead ID/name, CUI, selected label or mapped LeadBridge export; verified CUI-field mapping; account-bound authorization; agreed deadline and data-access limits. Preserve original identifier values and report invalid/ambiguous input rather than silently normalize it into a different target.
- **Selection output:** Selected lead IDs, label/filter identity, inspected filter conditions, retrieval interval, page/continuation accounting, duplicates, omissions, and a distinct completeness result. A 20–30-firm normal workload is not permission to truncate a larger selected set invisibly. Completeness is within the authorized visibility scope; these docs do not establish an atomic CRM snapshot.
- **Company output:** Linked CRM identities, raw CUI and mapping provenance, contacts, notes with IDs/attachments/timestamps, and explicit conflicts or unavailable data. Reusing CRM material does not establish its external factual truth or permit it to act as instructions.
- **Owned responsibility:** Mechanical selection, identity/context retrieval, deduplication, normalization, and coverage reporting only. No SWOT, digitalization advice, prospect sourcing, new business module, or output-publication decision.
- **Reuse:** Fetch shared organizations/persons/notes once per run where appropriate; keep account/entity identity and retrieval time attached. Persistent caching, storage format, freshness, invalidation, and privacy retention remain design choices. Pipedrive recommends caching and optimized v2 operations, but warns that v2 is not backward compatible with v1.[12]
- **Still not selected:** A full CRM mirror, broad SDK generation, or attachment-download/document-processing pipeline. Webhook versus other review/edit detection remains open. The earlier rejection of an automatic writer is explicitly superseded by the narrow earned-autonomy publisher under **Operation and delivery**; it is not part of this read-only preparation helper. Note comments are a separate API collection; whether to include them remains a context-scope decision.[5]

#### Security, reliability, and cost of the alternative

**MCP:** Use the necessary verified read tools for preparation, not blanket server approval. The future publisher needs a separately controlled note-create permission subject to **Operation and delivery**, not general CRM write authority. Claude Code enforces permission rules independently of prose instructions; deny takes precedence over allow, so a whole-server deny cannot be carved open with narrower read allows.[10] Inspect the actual tool inventory and review newly exposed tools. Separately control any shell/API path; MCP permissions do not automatically constrain a REST helper.

**REST:** Pipedrive documents an API token in the `x-api-token` header, tied to one user/company; one active token exists, and rotation can break existing integrations.[8] A GET-only helper limits its own actions, not the credential's inherent permissions. Scoped OAuth is another option: the documented read scopes cover leads/labels and contacts/fields/notes/filters; field-specific ItemSearch requires `search:read`.[11] Authentication choice, token storage/refresh, and revocation remain setup responsibilities; no credentials were requested or inspected here.

**Limits:** REST consumes a company-wide daily API-token allowance shared by API-token and OAuth integrations, with additional burst/search limits; daily exhaustion produces 429 until reset.[9] A helper needs bounded concurrency, timeouts, backoff, response validation, and explicit partial-result reporting—not unlimited retries. Neither REST documentation nor the public MCP tool list proves the five-minute single-company or 08:00 batch timing requirement. No measured MCP/API performance comparison has been run.

**Tradeoff:** A helper can make selection accounting, joins, payload reduction, and run-scoped reuse easier to control. It also adds mixed-version decoding, pagination validation, field-mapping changes, credential lifecycle, throttling, and maintenance. MCP-only avoids duplicating that integration when it already supplies the required packet. API-only would ignore the user's required official MCP connection and is not the recommendation.

#### Adoption and verification gate — future authorized work

1. Configure and authenticate the required endpoint in the selected Claude Code project/account context. Inspect granted access and the actual `tools/list` schemas; configuration alone is not a working connection.
2. Compare MCP against the same preparation contract: a label set spanning multiple pages; included/excluded/archived leads; supplied UUID and duplicate names; CUI-field mapping; missing/conflicting links; contacts; overlapping/multipage notes; invisible records; and timeout/rate-limit handling. Compare the collected ID set with an approved UI/export baseline. Do not send CRM writes to test their availability.
3. Assign a REST helper only where a specific benefit is demonstrated. Validate actual lead-pagination and field-search response shapes and reconcile requested/returned bulk IDs. Keep one output contract regardless of transport; existing business modules must not need to know which path supplied their input.
4. Exercise representative single-company and normal batch runs, including partial failure and unattended authorization refresh. Record actual elapsed time and completeness before claiming deadline compliance. Publication follows the now-defined Pipedrive/per-industry policy; transport, enforceable permissions, review/edit capture, and the supervised 08:00 deadline meaning still require verification/decisions.

**Assessment status:** Public-documentation comparison complete; load-bearing MCP tool/permission passages and REST operation/schema passages checked. Both delegated returns were recovered in full before integration. This is a scoped CRM-preparation assessment, not an audit of every CRUD endpoint. No connector setup, authenticated tool inventory, customer reads, API helper, scheduler, or external write was executed. The research conclusion is available; helper adoption and live capability/performance verification remain pending.

## Notes and Open Questions

### Confirmed language

- **Digitalization direction / angle:** Which business problem of this particular firm could be improved digitally, and why that angle fits. Not a product, technical implementation, or confirmed diagnosis by default.
- **Phase 1 business module:** One of the three explicitly named analyses. Shared acquisition/checking/presentation helpers are technical support, not extra analyses.
- **Hypothesis:** A clearly labeled inference grounded in actual firm activities, not an invented present process, loss, or complaint.

### Clarification history

The assistant's earlier proposal to map directions to named reference solutions was rejected. It is superseded by reference inspiration only, with no named solutions/technical details. The historical full-note volume must not be mistaken for today's requirement: only 20–30 selected firms need next-day research notes. Historical pipelines are background, not a target to repair.

### Current frontier

- **Runtime/integrations:** Claude Code on Windows initially, with cross-platform operation preferred. Specify concrete skill invocation and helper/tool integration without reopening the selected host.
- **Module dependencies — resolved:** Strict profile → SWOT → directions; each direction references a supported W/T. Pattern-library input, fact/hypothesis labels, and the no-W/T discovery fallback are confirmed. Exact presentation and novel unmatched-problem handling remain open.
- **Module invocation:** One preparation entry point with three internal modules is settled. Exact request syntax, dependency handling, and evidence-update behavior remain to be specified; separate user-facing module calls are not a Phase 1 obligation.
- **Presentation:** How do three analyses fit the two-minute reading view? What is shared or different before a call versus a first meeting?

### Retained dependent questions

- Exact lead/export adapters, identity/group/contact rules, and provenance/precedence for existing CRM material.
- Proposed module/data contracts need final compatibility and extension-registration details; dependencies and result ownership are no longer unspecified.
- Pattern/minute persistence and review permissions, evidence matching, novel unmatched W/T handling, and confidence scoring beyond auditable case support. Do not implement these during design.
- Research depth, source freshness, blocked-site handling, evidence sufficiency, contradictory data, and time/coverage conflict.
- Batch initiation, timezone/deadline conditions, retries, resumability, duplicates, and partial completion.
- Pipedrive destination and earned industry autonomy are settled for the Excel batch target. Draft/review storage, Excel mapping, final industry taxonomy, enforceable write permissions, CRM-edit capture, privacy, and persistence remain open; supervised deadline semantics and model/library-change requalification also remain open.
- Required Pipedrive MCP setup and live verification of the documented MCP/API responsibility proposal. The documentary assessment is complete; optional helper adoption, authentication, and live capability/performance checks remain pending.
- Semantic quality checks, useful human review, test cases, and acceptance of exercised behavior.
- Migration/reuse of historical artifacts without importing old templates, product catalogs, or obsolete workflow rules.

No later-phase business behavior is an unresolved Phase 1 obligation. Later modules are deferred; only the extension seam is in scope.

## Definition of Done

These are Phase 1 product outcomes, not claims that filling this brief completes the product. Concrete contracts and exercised acceptance conditions remain open.

- [ ] **Exactly three usable modules:** Company profile, SWOT analysis, and digitalization directions produce their agreed results; no additional business modules are included.
- [ ] **Company-grounded preparation:** Profile/SWOT/directions refer to the actual firm and preserve facts, hypotheses, and unknowns distinctly; generic industry-template substitution does not qualify.
- [ ] **Strict chain and fallback:** Directions consume SWOT and each references a supported W/T; all SWOT items have fact/hypothesis labels and evidence as specified. No supported W/T produces no directions and 2–3 discovery questions; missing upstream output is visibly blocked.
- [ ] **Auditable pattern feedback:** Industry seeds are unvalidated; one supplied minute can create/reinforce patterns, explicit nonconfirmation lowers support, unassessed items do not count as negatives, and replay/corrections do not double-count cases. Existing runs retain their input/library revisions.
- [ ] **La Butic calibration:** The public-only acceptance spec above passes after separately authorized execution, with private-case feedback held out and outputs frozen before oracle comparison. It has not been run here.
- [ ] **Correct reference boundary:** Commercial PDFs inspire business areas only; the internal minute is private feedback/calibration evidence, excluded from the public-only research run. Directions are not limited to those references or a portfolio and have no named solutions, technical implementation, or solution-specific client explanation.
- [ ] **Reuse and coverage:** Existing LeadBridge/Pipedrive information is used without unnecessary reacquisition. Every named source has an honest coverage outcome; blocked or unmatched is not reported as evidence of no problem.
- [ ] **Both request modes:** On-demand lead name/ID or CUI and batch Excel/label/export work under the agreed identity and duplicate rules. The confirmed earned-autonomy delivery target is Radu's Excel batch.
- [ ] **Timing:** The real workflow meets up-to-five-minute on-demand preparation and next-day 08:00 readiness for normal 20–30-firm batches under agreed start/access conditions; defined failure behavior is exercised too. Whether supervised readiness means reviewable drafts or posted notes remains explicitly unresolved under **Operation and delivery**.
- [ ] **Usable reading view:** Radu can scan the result in approximately two minutes before a cold call or first meeting and judge the analysis/angle useful on representative firms.
- [ ] **Extension boundary:** Inputs, outputs, dependencies, ownership, and addition procedure allow a later module to plug in without changing existing modules. The verification method must be agreed without building later-phase functionality now.
- [ ] **Safe earned-autonomy delivery:** Excel rows resolve to correct CRM leads and canonical industries; supervision, 20-note promotion, edit/rejection logging, posted-note demotion, visible labels, and deduplicated verified publication behave as **Operation and delivery** specifies. A new/unresolved industry cannot inherit autonomy. Credentials, privacy, edit capture, and final storage/transport contracts still need definition and exercise; no acceptance run is claimed here.
- [ ] **Pipedrive MCP prerequisite and CRM preparation:** Claude Code is configured and authorized against `https://mcp.pipedrive.ai/mcp`. The selected MCP or MCP-plus-API path verifies required lead/organization/contact/note reads and complete label-selected batch retrieval under one preparation contract. Any API helper has a documented demonstrated benefit and verified pagination/identity/context behavior; it does not replace the required MCP connection or change business-module responsibilities. Future writes follow the explicit policy under **Operation and delivery** and separately verified permissions. This is a future setup/verification requirement, not a completed connection.

## How Success Is Judged

Confirmed priorities are useful company-specific preparation, problem-level—not product-level—angles, reuse, modularity, and the specified preparation/reading windows. Business effects such as more booked meetings are not yet agreed measurable acceptance targets. A structurally valid note is insufficient: Radu's judgment of usefulness and evidence-grounded reasoning must matter. Exact review rubric and tradeoffs for sparse evidence or slow sources remain open.

## Sources

[1] https://code.claude.com/docs/en/skills — Claude Code: Extend Claude with skills
[2] https://code.claude.com/docs/en/mcp — Claude Code: Connect to tools via MCP
[3] https://support.pipedrive.com/en/article/mcp-tools — Pipedrive MCP tools - Knowledge Base | Pipedrive
[4] https://support.pipedrive.com/en/article/mcp-claude — Pipedrive MCP for Claude - Knowledge Base | Pipedrive
[5] https://developers.pipedrive.com/docs/api/v1/openapi.yaml — v1 OpenAPI
[6] https://developers.pipedrive.com/docs/api/v1/openapi-v2.yaml — v2 OpenAPI
[7] https://pipedrive.readme.io/docs/core-api-concepts-pagination — Pagination
[8] https://pipedrive.readme.io/docs/core-api-concepts-authentication — Authentication
[9] https://pipedrive.readme.io/docs/core-api-concepts-rate-limiting — Rate limiting
[10] https://code.claude.com/docs/en/permissions — Configure permissions - Claude Code Docs
[11] https://pipedrive.readme.io/docs/marketplace-scopes-and-permissions-explanations — Scopes and permission explanations
[12] https://pipedrive.readme.io/docs/guide-for-optimizing-api-usage — Guide for optimizing API usage
