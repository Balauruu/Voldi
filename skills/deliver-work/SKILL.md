---
name: deliver-work
description: "Use when delivering artifacts or closing tracked work."
version: 1.1.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [delivery, handoffs, verification, artifacts]
    related_skills: [github, grounded-citations, wayfinder]
---

# Deliver reachable work

Give the intended reader a verified route to every required result.
A completion message is navigation, not a substitute for substantive work.

This skill checks delivery; it does not authorize publication, create new
acceptance criteria, or implement a runtime hook. An adequate answer in
chat needs no manufactured report or file.

## When to Use

- Delivering reports, files, code, specs, research or creative outputs.
- Integrating delegated outputs into a final handoff.
- Requesting judgment on a multi-artifact prototype.
- Closing tracked work with deliverables.

Do not use to turn a simple answer into a documentation project, impose
a new approval process, or publish work merely to obtain a link.

## Prerequisites

No installation or external service is required for an in-chat handoff.

For files, trackers or publication, use the tools and authorized access
available in the current session. Check their current schemas before
choosing attachment, artifact or status fields; do not guess interfaces.

## Inputs

Obtain these from the request and existing task records before asking:

- **Required results:** the outcomes and substantive acceptance criteria.
- **Actual outputs:** complete answer text, files, records or delegated
  results to inspect—not merely a worker's summary.
- **Reader and surface:** who needs the work and where they will use it.
- **Delivery controls:** canonical home, requested format/schema, privacy,
  allowed destinations, publication permission and retention requirements.
- **Closure controls, if tracked:** exact task identifier, required
  acceptance or review gates, and authority to change its state.

Missing-input behavior:

- Retrieve an identified source or task record when authorized.
- Ask only if a missing answer changes access, permission, retention,
  required acceptance or the result itself.
- For an ordinary answer, use the current chat unless instructed otherwise.
- For an artifact, use the requested surface's supported delivery mechanism;
  do not infer permission to upload elsewhere.
- Name unresolved requirements rather than inventing destinations,
  permissions, approval or verification.

An owner-supplied schema or artifact-only return overrides the default
handoff format throughout this procedure, not only in the final message.

## Before Delegating — Only When Delegation Is Used

Give each worker:

- Required outputs and substantive acceptance criteria.
- Exact inputs and allowed output destinations.
- Intended reader, delivery surface and governing controls.
- This skill's delivery requirement.
- A requirement to return actual output paths/URLs and verification
  evidence, with remaining limitations—not only a summary.

Children do not automatically inherit the coordinator's persona or task
controls. The coordinator owns integration, verification and final delivery;
a worker's completion claim is not proof of an external write.

Done when: each assignment can be executed without guessing its output,
destination, permission boundary or completion conditions.

## Procedure

### 1. Account for the substantive result

Match every required outcome to its actual complete output.

Inspect meaning and usability, not just file existence or task status.
Check the findings, practical implications, attributable sources where
needed, relevant alternatives/tradeoffs and material limitations against
the task's own criteria.

For delegated or truncated returns, recover the complete substantive
content before assessing it. This includes long-line truncation and
shortened structured-field previews: decode and inspect the retained
complete record rather than accepting the preview as the deliverable.

Retain final outputs and necessary evidence; omit disposable logs.
Reconcile multi-output inventories and declared totals programmatically
using `execute_code` or `terminal`.

If an outcome is missing, recover or finish it within scope. If blocked,
deliver permitted partial results and identify the unresolved outcome;
do not describe the whole task as complete.

Done when: every required outcome has an inspected result or an explicit
unresolved condition.

### 2. Bind the authoritative delivery route

Choose one authoritative route for each output:

- The complete result directly in the requested destination; or
- A concise answer linking or attaching the complete result.

Link necessary supporting assets from the authoritative answer.
Distinguish current work from superseded drafts and historical reviews.

Follow the owner's canonical-home, privacy and retention requirements.
Preserve required files in an authorized durable location when continued
availability is required. Scratch/cache paths are temporary, not permanent
handoffs; do not silently invent a new storage location.

If a real destination limit requires split delivery, obtain agreement on:

- Which complete sections belong in the destination.
- Where the remaining full detail and evidence will be accessible.
- Any local-only access or retention limitation.

Delivery-format agreement is not substantive acceptance.
A planned destination is not a published destination.

Done when: every required output has an authorized route suitable for
its reader, with access and retention boundaries identified.

### 3. Deliver on the selected surface

Apply only the relevant branch.

| Surface | Delivery rule |
|---|---|
| Chat | Put the complete substantive answer in chat. Do not create files solely to satisfy this skill. |
| Hermes Desktop files | Use `MEDIA:/absolute/path/to/file` for verified local files. A printed path alone is not a native attachment. |
| Local/CLI | Provide the exact accessible path and a base for relative paths. Label local-only access. |
| Shared tracker | Follow its selected canonical home; put the required full text there and link permitted supporting assets. Local paths and `file://` URLs do not establish shared access. |
| Repository resources | Link a published ref and actual file, not an uncommitted working-tree path. Use a stable permalink when the referenced revision matters. |
| Kanban files | Use supported attachments or the current tool's `artifacts` field as applicable. Verify the recorded attachment/state rather than assuming the write delivered it. |

For wayfinding tickets governed by the sub-issue convention, place the
complete textual resolution and necessary context in the sub-issue body;
comments and the parent map point there. A separate report or substantive
comment is appropriate only when the task's convention or owner direction
selects that home.

An explicitly accepted local-only tracker handoff must state that boundary,
identify the local output, and deliver it through the owner's local surface.

Publication is a side effect. Do not commit, push or publicly upload just
to create a link. If an authorized remote write fails, preserve and deliver
allowed local results, disclose the failed remote handoff, and leave that
condition unresolved unless the owner approves a scoped exception.

Done when: each output is delivered through its authorized mechanism,
or the precise delivery failure is visible.

### 4. Verify the actual route

Use evidence appropriate to the route and result. Reuse relevant checks
for an unchanged output; do not substitute mechanical checks for quality.

**Local files**
- Resolve the path and confirm existence/readability.
- Inspect the substantive content and required sibling assets.
- Check load-bearing relative links and anchors.

**External writes**
- Read back the exact target after the write.
- Compare the complete substantive body/record with the intended result.
- Retrieve load-bearing reports and attachments using authorized access.
- State untested reader-permission limits: agent access does not prove
  every collaborator has access.

A successful write, HTTP 200 shell, attachment identifier or linked
summary without its supporting report is insufficient.

**Downloadable archives**
- Check the archive's integrity and reconcile its members with the
  required resource inventory.
- Resolve load-bearing links within the packaged layout.
- Include required empty directories when their presence is part of the
  artifact contract.
- Label historical backups and reviews so they cannot be mistaken for
  the current deliverable.

If a route fails, correct it within scope or report the unresolved
condition. Do not hide it behind an accessible summary.

Done when: the actual delivered targets and necessary supports are
checked, with remaining access limitations stated precisely.

### 5. Orient the reader and handle acceptance

For an ordinary handoff, provide the result and a clear route to its depth.

Before requesting judgment on a multi-artifact prototype, also state:

- The exact construction or change the owner is being asked to accept.
- What that acceptance permits and what it does not establish.
- A short inspection route naming actual files and sections, what each
  contributes, and which unresolved quality questions require judgment.

Keep this inspection route separate from the audit/evidence inventory.
Agent checks do not make a large bundle self-explanatory.

Do not reopen settled choices or ask the owner to repeat completed
verification. Do not invent human approval where the task requires it.

Create the Delivery Handoff using the following default structure.
Adapt it to the requested format; omit conditional sections when absent.

```markdown
[State the important result or change, without replaying the process.]

**Work:** [Provide named links/native attachments, or the complete
substantive answer when chat is the authoritative destination.]

**Verified:** [State the checks actually performed and their scope.]

**Remaining:** [Include only unresolved criteria, blockers or material
access/retention limits. Say what remains—not a generic disclaimer.]

**Decision needed:** [Only when required: state the exact acceptance
question, its consequences, and the short inspection route.]
```

For an artifact-only return, put required status or limitation information
inside the artifact or permitted metadata; do not append a second report.

Done when: the reader can find and use the work, understand material
limits, and act on any genuinely required decision.

### 6. Close Tracked Work — Only When Applicable and Authorized

Close only when:

- The task's substantive acceptance criteria are satisfied.
- Its required delivery route is verified.
- Any required acceptance/review gate is satisfied, or the owner has
  explicitly approved a scoped exception.

Record an exception's exact scope; it does not erase unrelated criteria.

After changing external task state, read back the exact target's status,
resolution and attachment references. Do not report successful closure
solely because the update tool returned success.

If any required gate remains open, use the appropriate review, blocked
or partial state rather than claiming completion. Delivery can be
verified while acceptance remains pending.

Done when: the tracked state matches the verified outcome and the final
handoff accurately distinguishes completed, pending and blocked work.

## Complete Illustrative Example

This fixture describes an in-chat proposal, not an executed policy change.

**Input**

“Propose a review policy in chat only. It must require peer review for
production behavior changes, permit documentation-only corrections without
peer review, and leave emergency exceptions for a later decision.
Do not create files or close the task.”

**Complete output**

**Proposed review policy**

- Production behavior changes require peer review before release.
- Documentation-only corrections may proceed without peer review.
- Emergency exceptions remain undecided; this proposal does not authorize
  bypassing review during an emergency.

**Verified:** The proposal covers both supplied rules and preserves the
unresolved exception. Its complete text is above; no file was created
and no task state was changed.

**Remaining:** The emergency-exception policy is unresolved.
This is a proposal, not an adopted or enforced policy.

## Pitfalls

- A short summary is not delivery of missing substantive work.
- A file path is not proof of access on another person's surface.
- A temporary file is not a durable handoff.
- A successful publication call is not a verified published result.
- Delivery-format approval is not approval of the work.
- Verified delivery is not satisfaction of a required acceptance gate.
- Archive integrity does not prove completeness or correct relative links.
- More reports, attachments or checks do not repair a poor substantive result.

## Final Verification

- Every required substantive outcome is inspected or explicitly unresolved.
- The intended reader has a checked route to the complete current result.
- Necessary sources, evidence, implications and limitations remain reachable.
- Publication, privacy, output format and retention controls are preserved.
- Local-only, temporary, unpublished and untested access are labeled.
- Applicable surface, archive and external-readback checks are complete.
- Any required owner decision is specific and remains pending until given.
- Tracked status matches verified completion, review or blockers.
- The final handoff follows the requested format without unnecessary duplication.
