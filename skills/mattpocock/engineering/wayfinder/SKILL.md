---
name: wayfinder
description: Chart decision maps and resolve their frontier.
version: 0.2.0
author: Matt Pocock (origin), Balauruu (local collaborator), Hermes Agent
license: MIT
platforms: [linux, macos, windows]
disable-model-invocation: true
metadata:
  hermes:
    tags: [planning, decisions, maps, human-in-the-loop, tickets]
    related_skills: [grilling, domain-modeling, prototype, repo-research, deliver-work, github]
---

# Wayfinder

Find the route to a destination through decision tickets, not slices of a build. The map is an index; each child's body holds its complete question and resolution. This is a domain-agnostic planning procedure, not a tracker adapter, scheduler or permission to implement.

Origin: Matt Pocock's Wayfinder; local revision with Balauruu and Hermes Agent. See [LICENSE](LICENSE).

## When to Use

- A loose idea spans sessions and still contains consequential decisions or investigations.
- Resume an existing map and take its next eligible decision, optionally a named ticket.
- Don't use for a small, already-clear task, an implementation backlog, or making a human's HITL choices for them.

## Inputs and authority

| Input | Missing-input behavior |
|---|---|
| Loose idea, or existing map reference and optionally a child reference | Ask for one of these; do not invent an effort. Resolve supplied identifiers in the specified tracker, not a guessed service. |
| Destination, hard requirements, exclusions and established owner answers | Retrieve existing direction first; ask about consequential gaps. Mark tentative framing as unconfirmed until the human confirms it. |
| Project orientation and current input paths, if the effort uses a project | Read its governing instructions and actual current inputs; do not assume a repository, clean checkout or committed snapshot. Non-project efforts need no repository. |
| Tracker's documented Wayfinding operations, target/project, claim identity and write authorization | Without these, use draft-only mode; identify the missing adapter/authority. No automatic setup or local tracker fallback. |
| Research sources/sections or bounded discovery brief; inspection order and pacing | Preserve named coverage. Undefined required scope, consequential ambiguity, missing access or authority blocks dispatch. Retain nonblocking unknowns; absent special pacing, use separately scoped eligible units in adapter order without widening the corpus. |
| Output/evidence homes and allowed changes | Default to conversation drafts and read-only discovery. Ask before persistent files, prototypes, probes, branches, publication or other side effects without an authorized destination. |

**Two storage modes:**
- **Draft-only (default when setup or authority is absent):** produce proposed map/ticket bodies in conversation, or in an explicitly authorized local draft home. Mark them unpublished. Draft names/links are not tracker IDs, assignments, closures or shared access. You may conduct the authorized live decision conversation, but cannot claim/dispatch tracked work or report a tracker resolution. A separately authorized local-only research invocation may use `repo-research` with owner-selected canonical/evidence/review homes and exclusive recorded local ownership; consume its complete reviewed return in the draft child without claiming tracker publication or closure. Missing tracker permission does not grant this research or file permission. Request setup only if the owner wants a working tracker; `skill_view(name="setup-matt-pocock-skills")` is a separately authorized setup workflow, not a prerequisite to drafting.
- **Authorized tracker:** use the existing adapter and exact approved target. Its operations must define map identity, child membership, labels/type mapping, claims, ordering/frontier, dependencies, body update and closure/readback. An explicitly configured local-markdown tracker qualifies only with actual paths and ownership/state/dependency conventions; this skill supplies no canonical tracker path. If a required operation is unsupported, stop that operation and obtain an agreed convention rather than silently inventing one.

Default boundary: planning. A spec, course outline, locked decision or in-place change can be the destination being planned; naming it does not authorize producing it. Carry execution into a map only through an explicit owner-approved Notes override naming the allowed actions, outputs and verification. A task ticket may perform a bounded prerequisite to a decision, not deliver the destination. Called skills' defaults cannot widen this authority.

## Conditional dependencies

Use actual `skill_view` calls. Load only the triggered branch and the installed skills named in map Notes. Missing skill/tool support blocks that branch; disclose it rather than imitating a tool result.

| Trigger | Required loads/tools and limit |
|---|---|
| Chart destination/frontier, or work a grilling ticket | `skill_view(name="grilling")` and `skill_view(name="domain-modeling")`. Scope the interview to destination/breadth discovery when charting, or the chosen child when working; do not resolve every map decision through one interview. Domain terms may be discussed, but glossary/ADR writes still need approved homes and authority. |
| Prototype ticket | `skill_view(name="prototype")`; load only its question-matched support. Produce a cheap artifact in an authorized home and obtain live human reaction. No production integration or branch publication by default. |
| Research ticket | Name the appropriate installed research skill in the assignment and require the worker's `skill_view(name="<actual research skill name>")`. For repository/skill mechanisms use `skill_view(name="repo-research")`; follow [references/research-handoffs.md](references/research-handoffs.md). Use actual available delegation tooling for independent workers; if unavailable, keep dispatch pending. |
| GitHub tracker operations | `skill_view(name="github")` plus this project's adapter; other trackers require their installed adapter/skill, not GitHub assumptions. |
| Resolution integration or final handoff | `skill_view(name="deliver-work")`; reachability adds to substantive acceptance and does not approve decisions. |

Invoke `/wayfinder` with a loose idea to chart, or an existing map reference and optional child reference to resume. Without a named child, the agent chooses the first eligible frontier ticket. The base procedure uses Hermes tools, not OS-specific shell commands; adapters/probes impose their own prerequisites. Resolve this skill's relative supports against its skill directory and project inputs against the actual supplied root.

In every human-facing reference, use the map or child's readable title, wrapping its actual route in that name where available. IDs, numbers and slugs remain adapter metadata and diagnostic details, not standalone substitutes for names.

## Procedure

### 1. Bind the effort and destination

For a loose idea, retrieve established owner answers, load the charting dependencies and confirm what reaching the end means. For resumption, read the map's complete low-resolution body, Notes and current child metadata; fetch related/closed child bodies and full relevant discussions when their detail is needed. Do not load every body just to orient, or treat a closed research ticket as owner adoption.

A request for a **new map** means a separate effort and child set, with applicable requirements carried into Notes and old issues linked only as history. Preserve old closures unless expressly authorized to reopen. Confirm any broadened destination and exclusions before publishing. If the route is already clear and fits one session, do not manufacture a map; state the available handoff and ask how to proceed.

Choose the mode now: **charting** proceeds through steps 2–3 for initial discovery and creation; **resumption** retains the existing map and child identities, skips initial recharting/creation, and proceeds to step 4. Use steps 2–3 during step 6 only for authorized newly surfaced questions. Reuse an existing child for the same question instead of creating a duplicate; never replace a live map with a freshly populated template merely to resume it.

**Done when:** the destination, current authority/storage mode, binding requirements, accessible inputs and remaining owner questions are explicit; a missing consequential answer is not silently supplied.

### 2. Classify the breadth and produce the map

Chart breadth-first: examine the whole destination before pursuing a favorite solution. Treat examples as symptoms, not exhaustive scope. For requested exploration, bound the audit/discovery and retain candidate features as unselected; an inventory does not authorize widening the required research corpus. Preserve hard requirements in Notes and in the responsible child criteria.

Classify each item exactly once:
- **Ticket:** an in-scope question can be phrased precisely now, even if blocked or unanswered.
- **Not yet specified (fog):** in scope, but not yet precise enough to ticket. Do not pre-slice it; one patch can later yield several tickets or none.
- **Out of scope:** beyond the destination by explicit scoping direction, not merely vague or inconvenient. It never graduates; revisiting it requires a redrawn destination and fresh effort.
- **Decisions so far:** accepted route resolutions, each indexed by its child title and one-line gist; detail remains in that child.

Create the Map body with these sections. Use `None` for empty sections and `Unknown — <missing input>` only where nonblocking. Open children belong to the adapter's child query, not a duplicate narrative backlog. Include a membership task list only if the adapter requires it as the child relationship fallback.

```markdown
# <Name the destination-oriented map>
## Destination
<State the agreed end condition in one or two sentences; label an unconfirmed draft.>
## Notes
- Mode/authority: <draft-only/unpublished, or authorized tracker and exact target>
- Boundary: <planning; or explicit execution override with allowed actions/output/checks>
- Requirements and exclusions: <binding owner requirements; no invented preferences>
- Inputs and history: <actual current paths/references and relevant prior named maps>
- Session skills: <installed skill names this effort needs, or None>
- Research: <required coverage, discovery boundary, order/pacing, evidence homes; omit if absent>
## Decisions so far
- [<Resolved child's full title>](<actual child-body route>): <one-line gist; distinguish findings from decisions>
## Not yet specified
- <In-scope suspected question and what must clarify it; no live-ticket duplicates>
## Out of scope
- <Excluded work and reason; link a closed child by title if one existed>
```

**Done when:** each known item has one disposition, requirements remain binding, and the map indexes rather than duplicates resolutions. In draft-only mode, the output says unpublished, not created on a tracker.

### 3. Produce children, then wire dependencies

Create one child per precise question, sized to the actual session's capacity rather than an assumed token budget. Use labels `wayfinder:map` and `wayfinder:<type>` through the adapter's equivalent fields. Allocate identities/create children first, then wire blocking edges in a second pass. Use native dependencies when supported: a prose list does not replace a UI-visible native relationship. Fall back to the adapter's body convention only when native blocking is genuinely unavailable. Verify membership, labels and edges after writes; do not pretend draft arrows are native links.

Create each Child ticket body with this structure. Tracker identity, assignee and lifecycle are adapter metadata, not fabricated body state. In draft-only output include `Publication: Unpublished draft` below the title.

```markdown
# <Name the question in readable language>
Parent: [<Map title>](<actual map route, or explicitly draft reference>)
Type: <research | prototype | grilling | task>; Participation: <AFK | HITL>
## Question
<State the single decision/investigation or prerequisite this child resolves.>
## Inputs and constraints
<List current input locations, applicable hard requirements, allowed changes/output homes.>
## Acceptance
<State observable resolution criteria; HITL includes the actual human response.>
## Dependencies
<List blockers by named link for context, or None; native/adapter edges are authoritative.>
## Resolution
<Pending. When ready replace with the complete resolution structure in step 5.>
```

Types and resolution boundaries:
- **Research — AFK:** documentation, APIs, repositories or local knowledge needed for a decision. Retrieve facts through a worker using the appropriate research skill. Findings/recommendations never settle owner choices.
- **Prototype — HITL:** a cheap concrete artifact to answer a look/behavior/structure question. Link the actual artifact; resolution needs the human's live reaction, not just artifact creation.
- **Grilling — HITL (default):** decision conversation using grilling and domain-modeling. The human speaks for themselves; never simulate or answer their side. Even delegated choice does not silently reclassify a HITL ticket: clarify a changed participation contract if needed.
- **Task — AFK or HITL:** authorized manual prerequisite with no remaining design choice. For human-only work give a precise checklist and verify completion. Record resulting facts and safe locations/references, never secret values. Do not treat access/provisioning authority as implicit.

**Done when:** every precise question has a child-shaped contract; tracker mode has verified real identities/membership/edges, while draft mode has only labeled proposed relationships.

### 4. Claim and work only eligible tickets

**Charting:** finish the map and proposed/verified child set. In authorized tracker mode, dispatch only eligible research, following the owner's order and pacing; charting hand-resolves no non-research ticket. Stop charting after recording dispatch/progress, integrating available accepted research only within that research boundary.

**Resumption:** if the owner named a child, assess that child and report its exact blocking/ownership condition rather than silently substituting another. Otherwise select the first frontier child in adapter order. New work requires **open, unblocked, unclaimed children**; unblocked means every actual blocker is closed under the adapter's dependency contract. A closed out-of-scope blocker whose facts are still required does not satisfy the question: repair the dependency/acceptance contract before proceeding. Nothing eligible may mean waiting/blocked work, not completion.

Continuation or integration of an already claimed unit is different from selecting new work. Verify the same effort, authorized driving identity, retained unit/session ownership, current dependencies/acceptance and absence of competing active execution before continuing. Preserve the verified claim rather than unassigning/reassigning it. A matching assignee alone does not prove exclusive session ownership; request coordination if exclusivity cannot be established. Never take another session's claim. The same rule governs returned research integration and pending HITL continuation.

In tracker mode, reconcile existing assignments before work. For new work, claim by assigning to the configured driving identity (or documented local ownership operation), then read back ownership and eligibility before substantive work or dispatch. For verified retained work, keep its existing claim and apply the continuation checks above. Assignee/ownership is the claim, not a comment. Do not overwrite a competing claim; readback alone is not an atomic lock when the adapter lacks one, so serialize overlapping coordinators or request coordination. Re-read current state before shared writes.

In draft-only mode, an explicitly requested draft decision conversation may proceed without a fictitious tracker claim; it cannot count as shared tracker work. Present HITL questions and **wait** for actual answers. If the human is absent, preserve Pending and the next question. For all modes, **resolve at most one non-research ticket per session**; unrelated research may continue with independently owned units. Record deferred work, never use the research exception to batch human decisions.

**Done when:** each active tracked unit has verified eligible ownership, each worker has exclusive outputs, and the chosen ticket has either supported complete results or an explicit pending/blocking condition.

### 5. Integrate one complete resolution and verify delivery

Load deliver-work. Recover the full worker return, including truncated portions, before judging acceptance. Check facts against load-bearing evidence; record an actual human's choices separately from retrieved facts and recommendations. In tracker mode, re-read the live child and preserve its question, unrelated new content and discussion. Mark superseded requirements with their reason instead of erasing history.

Replace `Pending` under `## Resolution` with the following complete structure. Use `None`/`Not applicable — <reason>` for genuinely empty sections, not omissions; unresolved acceptance keeps status pending. Findings must live in the child body, not only a comment, gist or evidence directory.

```markdown
### Result and authority
<State findings/confirmed choice/completed prerequisite, who actually answered, and draft/published/acceptance status.>
### Evidence and reasoning
<Load-bearing source locators or actual human statements; what was read/executed and what each supports.>
### Practical implications
<What downstream work can now rely on; distinguish fact from proposed adaptation.>
### Alternatives and tradeoffs
<Material alternatives and reasons considered; no invented approval.>
### Limitations and unresolved choices
<Access/verification limits, open owner choices, or None.>
### Downstream handoff
<Name the next consumer/ticket; exact inputs, outputs, permitted actions and stopping point.>
### Superseded requirements
<Identify changed prior requirements and authority, or None.>
### Verification and delivery
<Record actual acceptance checks, reachable asset routes and scope. In draft mode state unpublished/no tracker claim or closure; in tracker mode record full-body readback and pending closure/pointer checks until verified.>
```

If the complete body exceeds a real tracker limit or the owner requires another home, obtain an agreed canonical alternative before publication; do not truncate or move findings silently. Verify the exact published full body and load-bearing asset routes before closing; then read back the closed state. Append a **named child-body pointer** and one-line gist to the parent's Decisions so far, and read back that exact parent entry. Comments may record status and point to the body; they do not replace it. Verify every external change, including final verification-record edits, by exact target readback. Preserve full findings locally only in an approved home if publication fails; report finished-unpublished/pending, not complete shared delivery.

**Done when:** acceptance, full canonical content, assets, closure and named parent pointer are verified in tracker mode; or complete labeled draft bodies are delivered with publication explicitly pending. Evidence verification is not owner adoption.

### 6. Advance the frontier and stop at the boundary

Graduate newly precise fog into children (create then wire) and remove that patch from Not yet specified. Retain still-vague in-scope fog. Reconcile invalidated questions/edges under current authority, preserving history rather than deleting evidence by default. If a ticket proves beyond the destination, record why in its body, close/read back it in tracker mode and add a named pointer only in Out of scope, not Decisions so far; draft mode proposes that disposition without claiming closure. Do not resolve another non-research child while performing this maintenance.

A clear route means no unresolved in-scope questions, live children or fog; an empty frontier with blocked, claimed, failed-publication or unanswered HITL work is **not** done. For handoff, state the current mode, named map/child routes, result status, remaining blockers/fog, active research ownership, and the next eligible action or explicit stop. Deliver local-only artifacts as local-only, not shared links. Hand off to execution only when separately authorized; do not start it because the map is clear.

**Done when:** each changed item has an accurate disposition and verified writes, the handoff names reachable canonical routes, and this session ends after charting or its one non-research resolution, with research progress accurately retained.

## Complete bounded example (illustrative, test-only)

This is a hypothetical input/output fixture, not a real human decision, tracker write or enacted interview. Never use the fixture to supply an absent human's answer.

**Input:** Resume an existing conversation-only draft map, **Reading-group session format**, destination “Decide the first session's format, without arranging the event.” Planning only; invitations and scheduling excluded. Its only child **Choose discussion format** asks “Facilitated round or free discussion?”, is grilling/HITL, has no blockers, and requires the human's confirmed selection. Fog: “Other format choices, if the choice exposes any.” No tracker adapter, project, persistent output home or research scope is supplied. In the fixture's live exchange the human says: “Use the facilitated round so newcomers get a turn; a single facilitator is enough. Nothing else needs deciding about the format.” They confirm the restatement; no execution permission is given.

<a id="reading-group-session-format"></a>

**Complete output — updated draft map:**

```markdown
# Reading-group session format
## Destination
Decide the first session's format, without arranging the event.
## Notes
- Mode/authority: Draft-only, conversation-local, unpublished; no tracker adapter or write permission.
- Boundary: Planning; no event execution authorized.
- Requirements and exclusions: Give newcomers a turn. Invitations and scheduling excluded.
- Inputs and history: Supplied draft and this fixture's human exchange; no project or earlier maps supplied.
- Session skills: grilling, domain-modeling.
## Decisions so far
- [Choose discussion format](#choose-discussion-format): Human-confirmed facilitated round; draft resolution, unpublished.
## Not yet specified
None — the fixture's human confirmed no remaining format choices.
## Out of scope
- Invitations and scheduling: arranging the event is beyond this destination.
```

<a id="choose-discussion-format"></a>

**Complete output — updated child body:**

```markdown
# Choose discussion format
Publication: Unpublished draft
Parent: [Reading-group session format](#reading-group-session-format)
Type: grilling; Participation: HITL
## Question
Facilitated round or free discussion for the first reading-group session?
## Inputs and constraints
Supplied conversation draft and human exchange. Give newcomers a turn; no project changes, persistent files, invitations or scheduling authorized.
## Acceptance
The human confirms a format and whether further format choices remain; satisfied in this fixture's exchange.
## Dependencies
None.
## Resolution
### Result and authority
The fixture's human confirmed a facilitated round with one facilitator; the draft choice is settled, but unpublished. No real user decision is asserted by this example.
### Evidence and reasoning
Human statement in the supplied fixture: “Use the facilitated round so newcomers get a turn; a single facilitator is enough.” They confirmed the restatement. No research or executed artifact is claimed.
### Practical implications
A later event organizer can use a facilitated round as the format input; arranging the event remains unauthorized here.
### Alternatives and tradeoffs
Free discussion was the alternative. The human chose turn-taking for newcomers over unstructured participation; facilitation adds a role to arrange later.
### Limitations and unresolved choices
No tracker adapter or publication permission. No remaining format questions in the fixture; organizer and event details are outside scope, not hidden fog.
### Downstream handoff
Consumer: a future event-organizing effort. Input: this draft format choice. Output: an event plan only if separately requested. Permitted action now: hand back the draft; stop before invitations, scheduling or implementation.
### Superseded requirements
None.
### Verification and delivery
Compared the draft result to the supplied human exchange and acceptance criterion; no real execution or source research. The named map pointer routes to this complete conversation-local body. Unpublished; no tracker claim, closure, shared attachment or external readback is asserted.
```

**Complete output — session handoff:** Draft-only route clear for the fixture's destination. **Reading-group session format** indexes **Choose discussion format** above; both are conversation-local, not shared tracker records. One non-research draft choice was recorded from the hypothetical human input; no research is active, and no blockers or fog remain. Publication awaits an adapter and authority. Stop; arranging the event requires a new authorized effort.

## Pitfalls and verification

- Map completion is route clarity, not delivery of the destination. An execution override must be explicit, bounded and verified.
- Setup templates may still put answers in comments; this skill's child-body requirement governs Wayfinder resolutions. Document and authorize any needed adapter amendment instead of executing obsolete defaults.
- Verify exact target identity, child membership, eligibility, ownership and native edges; title links aid reading but do not replace tracker identities.
- Check complete question/resolution, evidence, implications, alternatives, limits and downstream inputs/outputs. Partial worker windows and ticket counts are not substantive coverage.
- Reconcile fog, live children, resolved route decisions and exclusions without duplication. Read back each external write and load-bearing asset before claiming closure/handoff.
- Keep HITL answers, AFK findings, recommendations, publication status and execution authority distinct. If any gate is missing, return a precise pending state, not a guessed success.
