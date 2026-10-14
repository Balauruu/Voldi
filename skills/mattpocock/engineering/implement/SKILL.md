---
name: implement
description: "Execute agreed work from specifications or tickets."
version: 1.0.0
author: Matt Pocock, Balauru, Hermes Agent
license: MIT
platforms: [linux, macos, windows]
disable-model-invocation: true
metadata:
  hermes:
    tags: [implementation, specifications, instructions, verification]
    related_skills: [test-driven-development, writing-for-agents, hermes-agent-skill-authoring, deliver-work]
---

# Implement agreed work

Actually implement the work authorized by a specification, tickets, or an explicit owner instruction. Deliver working code, usable agent-facing documents, reusable skill resources, or their agreed combination—not merely a plan. Companion skills supply branch-specific technique; current specifications and owner controls govern scope and acceptance.

## When to Use

- Execute agreed behaviors, revise approved resources, or finish a bounded implementation ticket.
- Resume implementation at the first unmet acceptance condition without erasing prior evidence.
- Don't use to select an adoption proposal, resolve an unapproved design choice, or turn planning/research into execution. A Wayfinder map plans by default; only its explicit execution override or a separate implementation instruction authorizes building the destination. Research findings are recommendations, not adoption approval.

## Prerequisites and inputs

Use the declared project/worktree and actual tool capabilities; this skill requires no new service, runtime hook, or global configuration. Resolve project-relative paths from that root, and this skill's relative reference from its skill directory.

- **Governing work:** approved specification or selected tickets with complete relevant discussion; an explicit bounded instruction in conversation can itself be the specification. Retrieve supplied references before editing. If scope or intended result is absent, ask; do not invent a spec or silently begin planning instead.
- **Authority and controls:** governing repository/project instructions, current owner overrides, reserved decisions, allowed edits, protected inputs, and publication/activation limits. Resolve conflicting consequential controls with the owner before the affected action. Overrides apply only to the roles/phases they name.
- **Current resources:** actual source files and supporting inputs, including relevant dirty/untracked work. Read the whole resource being revised and applicable supports. Retrieve missing context; ask if an inaccessible required input prevents correct implementation, otherwise report the bounded unknown.
- **Acceptance seam:** required observable behaviors, permitted test inputs, independent contexts or owner review when specified, and applicable test commands. Establish task-appropriate checks from approved requirements and project conventions; ask before guessing a consequential seam or changing acceptance.
- **Delivery:** requested artifacts, permitted file destinations or inline return, audience, and canonical home. Use conversation for an explicitly inline resource; resolve a required but ambiguous write destination before saving.
- **Optional continuation evidence:** last verified phase, input/output bindings, worker handles, checks and preservation evidence. Absence does not block fresh work; on resumption it limits which prior claims can be carried forward.

## Ordered workflow

### 1. Bind the execution contract

**Input:** governing work, current controls and resources. Read them using `read_file`, `search_files`, or the authorized retrieval tool; inspect relevant worktree status with `terminal` when Git is involved. Extract every material requirement, allowed destination, protected boundary, acceptance condition and owner-held decision.

**Retain:** a proportional requirement → action/output → check map in working context or an authorized handoff; no extra contract file is mandatory. Mark unresolved values explicitly rather than manufacture owners, approvals or paths.

**Done when:** scope, inputs, outputs and checks are identifiable; blocking gaps are exposed before affected edits. Independent authorized work may continue, but a missing prerequisite cannot be disguised as completion.

### 2. Select discipline and preservation checks

**Input:** the contract and actual deliverable contents. Classify each part as code, ordinary agent-facing document, reusable skill resource, or mixed; record the classification and verification seam.

- **Code:** load `test-driven-development` by its verified name; do not assume `/tdd` exists. Apply test-first work where possible at the pre-agreed code seams. If it is unavailable or unsuitable, retain the agreed observable checks and disclose the limitation; ask only if a consequential change to agreed acceptance is needed. Companion examples do not authorize commits or deletion of unrelated existing work.
- **Ordinary prompts, handoffs and contracts:** load `writing-for-agents`. It owns document construction, not reusable skill packaging.
- **Reusable skills and bundled reference files and templates:** load `hermes-agent-skill-authoring`. For an authorized live user-local change, `skill_manage` targets the selected profile; for in-repo files use `write_file`/`patch` in the actual repository. For a proposal-only or staged revision, use `write_file`/`patch` only in its authorized candidate directory, leaving installed profiles unchanged. Apply its construction/validation standards to user-local work without importing repo-only layout, docs regeneration, test files, commits or PR steps into either live edits or proposals.
- **Mixed work:** apply each branch only to its own artifacts, then test their agreed interface together. Markdown does not become code merely because it sits in a software repository.

For this workflow, import the TDD companion's RED–GREEN–REFACTOR technique at applicable agreed test-first seams. This skill's conditional applicability, focused-check/final-suite cadence and preservation boundaries govern; blanket companion approval, deletion/restart and per-cycle full-suite policies do not add requirements. Preserve an explicit task requirement for strict TDD or a different cadence, and obtain owner direction before changing that control. Never delete retained or unrelated work just to manufacture a fresh test-first history.

Inspect ordinary revision boundaries through focused diffs and substantive comparison of meaning, facts, attribution, interfaces and behavior. Explicit literal copying or immutable inputs require a targeted direct comparison; hashes serve only a concrete integrity or identification need. Do not impose byte identity on editorial repairs.

**Retain:** branch/check choices and the baseline needed for the actual protected boundary. Before protected-input, collateral-risk, interrupted, concurrent or asynchronous work, read [references/recovery-and-preservation.md](references/recovery-and-preservation.md) for bounded preservation, handoff and recovery operations.

**Done when:** every artifact has a fitting discipline, edit boundary and check without unauthorized machinery.

### 3. Implement the smallest complete agreed slice

**Input:** the requirement map, current resources and branch/check choices. Use `patch` for targeted edits or `write_file` for authorized full resources; read existing files first.

- **Code:** when test-first is applicable, write a behavior test at the agreed seam; run it with `terminal` and verify the expected failure before production implementation. Make the minimal change, run that test to green, then refactor without changing behavior. Otherwise exercise the agreed observable seam and explain the test-first limitation. Repeat per complete slice, retaining actual commands/results. Run applicable typechecking and focused test files regularly, then the full applicable suite at the end. Explain unavailable checks and blockers honestly.
- **Instructions/resources:** construct the approved document or skill itself, including required inputs, ordered actions, concrete output structure, missing-input behavior and completion checks. Keep editorial judgments in instructions. Bundle required supports and verify relative pointers. Authoring is not permission to execute the resource's target task.
- **No prose machinery by default:** add executable helpers only where explicitly authorized. Do not manufacture deterministic runners, validators, state controllers or a test framework to make a document task resemble software development. Authorized packaging checks remain mechanical checks, not behavioral proof.
- **Mixed:** implement the agreed code/resource binding, then exercise the actual consuming interface; isolated passes alone do not prove that interface works.

Historical or archived tests are context, not acceptance authority for a fresh resource design. Preserve approved constraints rather than redesign them to fit old fixtures.

**Retain:** complete authored artifacts and observed slice-level checks, bound to the exact revision tested.

**Done when:** the slice exists in the permitted destination and applicable local checks pass, or a blocker identifies the unmet condition. Continue remaining authorized slices; do not stop at a stub or plan.

### 4. Exercise the observable behavior

**Input:** current artifacts, agreed seam and required real inputs. Run the specified behavior, inspect actual outputs, and compare every acceptance condition with the result. For instruction-led work, use real agent handoffs at the required seam; give the consumer only declared resources/inputs, its role, permitted edits, output home and completion conditions.

Use an independent worker context when required; an author's self-review cannot substitute. Do not add hidden design history that masks missing consumer inputs. If no authorized independent route is available, disclose the blocked gate. Respect read-only or authoring-only boundaries; request separate permission for execution outside them.

**Retain:** actual consumed bindings, inputs, returned artifacts, located failures, repairs and rerun results. Separate authored resources, mechanical checks, real behavior and owner acceptance. An illustrative example or synthetic fixture is not required real-input execution.

**Done when:** every applicable seam is exercised on the current revision and produces the required result, or the unmet gate and evidence limit remain explicit.

### 5. Review scope and acceptance

**Input:** requirement map, current diff/artifacts, checks and preservation evidence. Account for every requirement; inspect substantive quality and protected boundaries. Resolve load-bearing findings and rerun affected checks after revision. Compare code and instruction interfaces in mixed work.

Use [references/recovery-and-preservation.md](references/recovery-and-preservation.md) when continuing prior phases, handling delayed returns, or carrying preservation claims forward. Do not repeatedly report a pending worker without establishing actual status.

**Retain:** requirement dispositions, current verification and unresolved owner decisions. Owner approval remains “Not requested”, “Pending”, or the actual recorded decision; never infer it from a test pass or ticket closure.

**Done when:** all implementation/check criteria are satisfied, or blockers and unverified intervals are explicit. Required owner acceptance stays outstanding until the owner supplies it.

### 6. Deliver artifacts and return implement-result

**Input:** complete artifacts and reviewed dispositions. Load `deliver-work`; provide the full readable resource, not a summary in its place. Save only in declared homes; for inline delivery include the entire requested resource before the result record. Resolve local references and inspect substantive targets; label local-only access. Read back any separately authorized external write before claiming success.

Never automatically commit, publish, install, activate, deploy, close tickets or change runtime/configuration. Those actions need explicit authorization for their specific scope; permitted implementation edits alone do not grant it.

When no controlling return format is supplied, create **implement-result** with these sections, inline by default or at an owner-requested report path. Replace guidance with observed facts; no extra file is required. Keep every default section, using “None” for empty sets and “Not applicable” only with a reason. An owner-supplied schema/template or exclusive artifact-only return overrides this default: put applicable information in its permitted fields or existing canonical report without adding another deliverable. Retain evidence only in already authorized execution context/handoffs when the return excludes it.

```markdown
# implement-result
## Status
[Complete | Blocked | Partial: state the acceptance boundary met or unmet.]
## Authority and inputs
[Name the governing spec/tickets, owner controls, current input bindings and branch(es).]
## Artifacts
[List every complete deliverable with exact path/base or inline section; distinguish local-only and published.]
## Checks
| Criterion | Kind | Evidence and observed result |
|---|---|---|
| [Actual requirement] | [Mechanical / Behavior / Review] | [Actual command, return or located inspection; label not run and why] |
## Preservation
[State semantic/literal checks and their scope, or why not applicable; expose unverified intervals.]
## Owner acceptance
[Actual recorded decision, Pending, or Not requested; distinguish it from verification.]
## Blockers and next action
[Each unmet condition, evidence limit and who must supply what; “None” only if none remain.]
```

**Retain:** reachable full artifacts plus this return, including necessary authorized recovery evidence.

**Done when:** every requested outcome is reachable and honestly classified. “Complete” means all required checks and acceptance conditions are met; use “Partial” when useful work is delivered but any required gate remains unmet, naming the blocked phase, and “Blocked” when a prerequisite prevents meaningful implementation progress.

## Complete illustrative input/output — not actual execution

**Input:** “Write an inline handoff instructing an agent to extract only recorded venue decisions from supplied notes. If notes are absent, ask before extraction. No booking or publication. Authoring and semantic inspection only; no independent exercise or owner approval required.” This bounded request is the spec.

**Complete output:**

```markdown
# Extract recorded venue decisions
## Purpose and scope
Return recorded venue decisions from supplied notes. Do not choose a venue,
book anything, contact participants or publish the result.
## Inputs
Use the notes supplied in the assignment. If absent, ask before extraction.
## Procedure
1. Read the notes and extract only explicitly recorded venue decisions.
   Done when: every recorded venue decision is accounted for without inference.
2. Return the decisions using the output below.
   Done when: all extracted decisions are included, or absence is explicit.
## Output
Return “Venue decisions” followed by one bullet per recorded decision.
If the notes contain no venue decision, return “Venue decisions: None recorded.”
## Checks and stopping
Compare each decision with the notes; invent no choice or booking confirmation.
Stop after returning the extraction. Missing notes block extraction, not authoring.

# implement-result
## Status
Complete: the requested inline handoff is authored and semantically inspected.
## Authority and inputs
The bounded inline request above; ordinary-document branch; no source file supplied.
## Artifacts
Complete inline handoff: “Extract recorded venue decisions”; no file or external write.
## Checks
| Criterion | Kind | Evidence and observed result |
|---|---|---|
| Extract recorded decisions only | Review | Procedure step 1 and final check prohibit inference. |
| Ask when notes are absent | Review | Inputs require asking before extraction. |
| No booking or publication | Review | Scope prohibits both; stopping limits the task to extraction. |
| Independent behavioral exercise | Behavior | Not run: the illustrative request authorizes authoring and inspection only. |
## Preservation
Semantic inspection retains every supplied constraint. Literal integrity is not required;
no existing file was revised and no earlier interval is claimed.
## Owner acceptance
Not requested: the supplied acceptance boundary requires authoring and inspection only.
## Blockers and next action
None for this authoring scope. A future extraction requires that run's notes.
```

These filled review statements teach the return format within invented premises; they are not evidence of executing the proposed workflow.

## Pitfalls and verification

- A plan, valid frontmatter, existing file or worker's success claim is not completed implementation. Inspect the actual deliverable and behavior required by the contract.
- Verify coverage, scope, factual/semantic preservation, current revision bindings, usable pointers and actual test results separately.
- Never substitute fixtures, delayed phase reports, inferred approval or mechanically green packaging for the acceptance seam the owner specified.
- If a required input, permission, tool, independent consumer or delivery route is unavailable, retain useful permitted outputs and expose the blocked gate; do not fabricate results or claim completion.
