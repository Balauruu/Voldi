# Conditional research handoffs

Read this only when a Wayfinder research ticket is being scoped, dispatched or integrated. The map remains the parent; research does not create another map, settle owner-dependent decisions or authorize implementation.

## 1. Bind coverage and pacing

Use the parent's Notes and the child's complete question/acceptance criteria. For a named corpus, account for every required repository, resource, section and appendix; separate required inspections from duplicates, supporting-only sources and discovery leads. Use `execute_code` for deduplication/count reconciliation when the corpus contains multiple entries. New leads need a scope-change proposal, not silent additions to the mandatory queue.

Honor the owner's inspection order and pacing. “One repository at a time” means claim and dispatch one independently scoped inspection, leaving the rest unclaimed. Where concurrency is requested, independent ownership means distinct units and attribution, not necessarily serial scheduling. Run only actually eligible units within the real runtime limit; true prerequisites block work, unrelated inspections do not. A required synthesis depends on all mandatory accepted inspections or explicit accepted scope exceptions, not the fastest reports.

For repository/skill mechanisms, load `skill_view(name="repo-research")`. Its interface reuses Wayfinder's map/research children and preserves HITL gates. It requires `grounded-citations` mechanism-inspection support and independent review; load `dynamic-workflow` only when its substantial-campaign scheduling trigger applies. A single supported inspection does not require that scheduler. Follow actual loads and full-result gates rather than copying scheduling machinery here. Outside repository mechanisms, name a suitable installed research skill and its task-specific acceptance checks. If no suitable skill, worker route or required independent reviewer is available, retain the affected branch as pending and request that prerequisite.

**Done when:** required coverage, scheduling order, dependencies, research skill and review boundary are explicit; every dispatch candidate has an answerable scoped question.

## 2. Claim before dispatch; isolate worker outputs

In authorized tracker mode, read current child state, verify native dependencies and claim ownership through the configured adapter, then read back the claim before dispatch. For an already claimed retained unit, use main Procedure step 4's continuation rule instead of inventing a new claim. Skip competing assignments. The coordinator alone writes shared claims, canonical tracker bodies, closures and parent pointers. Workers own only assigned exclusive outputs, with separate reviewer homes.

Without a tracker, local research is a **separately authorized `repo-research` invocation**, not an implied property of a draft map. Require explicit research/delegation permission, owner-selected canonical/evidence/reviewer homes and recorded exclusive local ownership. Use labeled draft parent/child references in the assignment and mark tracker claims/closure/publication not applicable. Follow all local-only review/evidence gates; consume the complete accepted-local or pending return in the draft child with an honest status. Do not fabricate an adapter or request remote publication permission merely to inspect sources locally.

Use the actual current input checkout if it contains uncommitted/untracked inputs; a newly created branch does not inherit dirty work. Create a throwaway `research/<name>` branch/worktree only when allowed by project placement rules and separately authorized/useful. Otherwise use the approved exclusive evidence home; research need not create commits or worktrees. Record the concrete input revision/dirty-state binding and evidence context in the child body without pretending local assets are published.

Create the Research assignment with the following fields, replacing all guidance with actual inputs. Deliver it through available delegation tooling; do not invent a `delegate_task` call or active handle if the tool is absent.

```markdown
# Research assignment: <Child's full title>
## Canonical context
- Parent: <Map title with actual route, or explicitly labeled draft reference>
- Child: <Research title with actual body route, or explicitly labeled draft reference>
- Ownership: <verified tracked claim/eligibility/readback, or authorized exclusive local assignment; no fictitious tracker claim>
- Research skill: <actual installed name>; load with skill_view(name="<that name>").
## Question and required inputs
<Complete question, acceptance criteria and binding owner requirements; include the actual map/child context, not only their titles.>
<Exact required source set/sections or discovery boundary, current target-input paths/revisions and known dirty-input limitations.>
## Ownership and permissions
<Exclusive evidence-output path and separate review home; allowed reads/writes/probes and explicit forbidden actions.>
<No shared tracker writes, destination implementation, installation or owner-decision selection.>
## Complete return
<Return the whole child-shaped resolution: findings, located evidence, practical implications, alternatives/tradeoffs, limits, downstream handoff and verification.>
<Canonical textual home is the tracked child body, or the separately owner-selected local research home consumed by the draft child. Evidence assets support it; no competing unauthorized report. Identify complete-return carriers if output is truncated.>
## Review and stopping
<Required independent review scope/checks, actual consumer and agreed stopping point.>
<Distinguish read material, executed probes and proposed adaptations. Stop at research findings, not adoption.>
```

**Done when:** each dispatched worker has verified tracked or recorded exclusive local ownership, complete accessible inputs, exclusive outputs, an actual research skill and a full canonical-return contract.

## 3. Recover, review and publish complete findings

Follow the delegation tool's actual delivery contract. If a turn must end for async results, record real progress and end it; polling transcript files does not replace completion delivery. Distinguish active handles, complete returns, review-pending, finished-unpublished and published work. On resume, reconcile current tickets and retained outputs; do not assume old worker handles are still alive.

Recover the full return before integration. Verify load-bearing evidence and retain requested independent review, including repairs/re-review of consequential errors; coordinator self-check is not independent review. For repo-research, follow its complete reading, citation/evidence and synthesis gates. A mechanical pass or closed ticket is not semantic acceptance or owner adoption.

In tracked mode integrate through main Procedure step 5: preserve the original question and live unrelated edits, publish complete findings in the child's body, read back full body/assets, close/read back, then update/read back the named parent pointer. Preserve repo-research's richer construction, warrants, rights, review/gate records and Sources under the child's Resolution rather than compressing them to a gist. A real body limit needs an agreed canonical alternative. In authorized local-only research, verify the selected complete canonical resource and incorporate its route/status into the draft child/map; tracker operations remain not applicable and the map remains unpublished. Access-limited or missing-gate results remain pending without an explicit scoped exception.

Retain resumable progress in an existing authorized campaign record or conversation: named required units, ownership, actual inputs, complete-return/evidence paths, reviews, publication/readback, blockers and next eligible work. Do not create a new persistent progress system without permission.

**Done when:** accepted findings are reachable from their named child bodies and parent pointers, every required unit has a truthful disposition, and owner choices/implementation remain unapproved.
