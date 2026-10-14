# Recovery, preservation and asynchronous handoffs

Read this reference before implementation involving expressly protected inputs,
a concrete collateral-edit risk, concurrency, asynchronous workers, or resumption.
It refines the main workflow; it adds neither execution authority nor mandatory
persistent snapshots. Resolve task paths from the declared project/worktree.

## 1. Establish the bounded preservation scope

**Input:** current contract, affected resources, unrelated dirty/untracked work and
named historical inputs. Identify permitted edits, protected existing artifacts,
and independently authorized output additions. Record the baseline needed to
compare the actual boundary in allowed execution output or handoffs.

Do not sweep dependency trees, Git internals or runtime caches into the scope
unless the task calls for them. Distinguish mutations of protected existing files
from unexpected creation; an authorized fixture is not a protected-file mutation.
Git HEAD omits previously dirty and untracked inputs: use the actual phase baseline.

**Retain:** protected/permitted sets, baseline bindings and bounded comparison evidence.
**Done when:** protection covers the task's real risks without inventing an inventory.

## 2. Verify and retain evidence for the phase

**Input:** phase baseline, current edits and actual preservation requirements.
Inspect focused diffs and protected meaning, facts, attribution, interfaces and
behavior. For literal copying or immutable inputs, make the targeted direct
comparison required by that contract; use hashes only for a concrete integrity or
identification need. Neither hash equality nor file presence establishes usability.

Retain the last verified phase, exact resource/input/output bindings, applicable
checks and preservation evidence in authorized output or handoffs when recovery
matters. Honor limits on persistent snapshots; do not create one automatically.

**Retain:** phase-scoped evidence, including allowed edits and unresolved checks.
**Done when:** preservation claims name their scope and supporting comparison.

## 3. Give concurrent workers explicit boundaries

**Input:** verified contract, actual resources and permitted output homes. Supply
each worker its role, complete inputs, exclusive permitted edits, protected inputs,
independently authorized additions, acceptance seam and full-return requirement.
Keep shared integration owned by the coordinator; do not let workers infer a
shared write authority from a common input directory.

Use the tool's actual async delivery contract. While a worker is genuinely active,
honor that handoff; do independent authorized work if available, or yield when
completion arrives only between turns. Do not poll transcripts/files as a substitute
for delivery or manufacture busywork. Never call a worker active without evidence.

**Retain:** assignments, handles, resource bindings and delivered full outputs.
**Done when:** active, returned and integrated phases are distinguishable, with no
ambiguous write ownership or fabricated completion report.

## 4. Resume at the earliest unsatisfied acceptance seam

**Input:** current canonical task state, live controls, retained phase evidence and
actual worker status. Read current state first, reconcile changed inputs/contracts,
and honor explicit reread requirements before carrying prior verification forward.
Recover the last verified phase, bindings and task-required preservation evidence;
account for earlier authorized edits rather than compare everything with HEAD.

If no worker is live, recover declared outputs and available completion evidence;
file presence alone is not completion. Inspect full outputs and substantive checks
before dispatching missing work. Do not restart completed stages merely because
handles are gone: that can overwrite evidence and waste owner attention.

Check retained execution output and phase handoffs before declaring evidence lost.
If recovery genuinely fails, name the unverified interval; establish comparison
evidence only for subsequent work. Current contents cannot prove prior preservation.
Treat delayed returns as reports about their assigned phase, not authority to
regress a later verified state. Revalidate affected seams when bindings have changed.

**Retain:** reconciled state, recovered evidence, unverified intervals and next unmet gate.
**Done when:** continuation follows actual current state rather than stale reports,
unsupported preservation claims or an assumed pending dependency.
