# GitHub Issues Management

Create, search, triage, and manage GitHub issues. Each section shows `gh` first, then the `curl` fallback.

## Prerequisites

- Authenticated with GitHub; read [auth.md](auth.md) if authentication needs setup or repair.
- Inside a git repo with a GitHub remote, or specify the repo explicitly

### Setup

```bash
if command -v gh &>/dev/null && gh auth status &>/dev/null; then
  AUTH="gh"
else
  AUTH="git"
  if [ -z "$GITHUB_TOKEN" ]; then
    if _hermes_env="${HERMES_HOME:-$HOME/.hermes}/.env"; [ -f "$_hermes_env" ] && grep -q "^GITHUB_TOKEN=" "$_hermes_env"; then
      GITHUB_TOKEN=$(grep "^GITHUB_TOKEN=" "$_hermes_env" | head -1 | cut -d= -f2 | tr -d '\n\r')
    elif grep -q "github.com" ~/.git-credentials 2>/dev/null; then
      GITHUB_TOKEN=$(uv run python "${HERMES_HOME:-$HOME/.hermes}/skills/github/github-auth/scripts/git-credential-token.py")
    fi
  fi
fi

REMOTE_URL=$(git remote get-url origin)
OWNER_REPO=$(echo "$REMOTE_URL" | sed -E 's|.*github\.com[:/]||; s|\.git$||')
OWNER=$(echo "$OWNER_REPO" | cut -d/ -f1)
REPO=$(echo "$OWNER_REPO" | cut -d/ -f2)
```

---

## 1. Viewing Issues

**With gh:**

```bash
gh issue list
gh issue list --state open --label "bug"
gh issue list --assignee @me
gh issue list --search "authentication error" --state all
gh issue view 42
```

**With curl:**

```bash
# List open issues
curl -s \
  -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/$OWNER/$REPO/issues?state=open&per_page=20" \
  | python -c "
import sys, json
for i in json.load(sys.stdin):
    if 'pull_request' not in i:  # GitHub API returns PRs in /issues too
        labels = ', '.join(l['name'] for l in i['labels'])
        print(f\"#{i['number']:5}  {i['state']:6}  {labels:30}  {i['title']}\")"

# Filter by label
curl -s \
  -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/$OWNER/$REPO/issues?state=open&labels=bug&per_page=20" \
  | python -c "
import sys, json
for i in json.load(sys.stdin):
    if 'pull_request' not in i:
        print(f\"#{i['number']}  {i['title']}\")"

# View a specific issue
curl -s \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/issues/42 \
  | python -c "
import sys, json
i = json.load(sys.stdin)
labels = ', '.join(l['name'] for l in i['labels'])
assignees = ', '.join(a['login'] for a in i['assignees'])
print(f\"#{i['number']}: {i['title']}\")
print(f\"State: {i['state']}  Labels: {labels}  Assignees: {assignees}\")
print(f\"Author: {i['user']['login']}  Created: {i['created_at']}\")
print(f\"\n{i['body']}\")"

# Search issues
curl -s \
  -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/search/issues?q=authentication+error+repo:$OWNER/$REPO" \
  | python -c "
import sys, json
for i in json.load(sys.stdin)['items']:
    print(f\"#{i['number']}  {i['state']:6}  {i['title']}\")"
```

### Reading a complete issue map before decisions

1. Read the requested ticket and its full thread, identify its parent map, then read the parent's body and discussion. Classify the actual deliverable before choosing an implementation workflow; a decision ticket needs a live owner interview, not an automatic PR.
2. Discover native children rather than assuming the parent's prose lists the current set:
   `gh api --paginate repos/OWNER/REPO/issues/PARENT_NUMBER/sub_issues --jq '.[] | {number,title,state,comments}'`.
   Read the parent summary with
   `gh api repos/OWNER/REPO/issues/PARENT_NUMBER --jq '{number,sub_issues_summary,comments}'`.
   Deduplicate child numbers programmatically and compare the collected count with `sub_issues_summary.total`; resolve any mismatch before claiming complete coverage. Inspect open and closed children, because closed research and downstream tickets can contain constraints absent from the active decision ticket.
3. Fetch each relevant child's body and complete comments with
   `gh issue view CHILD_NUMBER --repo OWNER/REPO --json number,title,body,comments,state,url`.
   For an owner-requested whole-map read, fetch every discovered child. Follow referenced resolution records and required local inputs; keep settled owner choices, evidence, proposals, pending decisions and execution claims distinct. Read live dependency endpoints when readiness matters, rather than treating labels or narrative status as proof.
4. Preserve long responses before interpretation. Batch independent reads, but reduce discovery responses to identities/counts and retrieve substantive bodies in manageable batches. For large programmatic reads, capture the full `gh` stdout with `subprocess.run([...], capture_output=True, text=True, check=True)` and parse that value before printing; do not parse an already clipped terminal preview. On resumption, compare fetched bodies, discussions and dependencies with the latest fully inspected records, including verified readbacks of your own intervening writes; an older pre-publication record produces false change alarms. Print a compact change inventory, then read required full content in bounded per-ticket batches. Size each batch by serialized body/thread length before printing rather than by ticket count; a few long resolutions can exceed the output limit. Split oversized records at section or comment boundaries and track unread portions. If output spills despite that check, page the returned full-output file through the omitted content instead of refetching and dumping the same records again. Print each long body only once per inspection; combining a changed-body dump with another full dump can bury unread obligations in truncation. Equality or a diff is navigation, not a substitute for a required full reread. If the work needs retained inputs, write readable per-ticket context copies through the file-writing tool under the project's permitted task workspace, then read their full contents with pagination. Counts, titles and file existence establish acquisition, not comprehension.
5. Check coverage against the native inventory and declared comment counts. Retrieve paginated comments when needed; any truncated or unread section remains outstanding. Delegate substantial read-only artifact comparisons with exact inputs and constraints, then integrate and verify consequential findings before asking evidence-dependent questions. Do not open the interview while that prerequisite work is still pending, because a premature question can mistake a research proposal or sibling constraint for a settled choice.

## 2. Creating Issues

**With gh:**

```bash
gh issue create \
  --title "Login redirect ignores ?next= parameter" \
  --body "## Description
After logging in, users always land on /dashboard.

## Steps to Reproduce
1. Navigate to /settings while logged out
2. Get redirected to /login?next=/settings
3. Log in
4. Actual: redirected to /dashboard (should go to /settings)

## Expected Behavior
Respect the ?next= query parameter." \
  --label "bug,backend" \
  --assignee "username"
```

**With curl:**

```bash
curl -s -X POST \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/issues \
  -d '{
    "title": "Login redirect ignores ?next= parameter",
    "body": "## Description\nAfter logging in, users always land on /dashboard.\n\n## Steps to Reproduce\n1. Navigate to /settings while logged out\n2. Get redirected to /login?next=/settings\n3. Log in\n4. Actual: redirected to /dashboard\n\n## Expected Behavior\nRespect the ?next= query parameter.",
    "labels": ["bug", "backend"],
    "assignees": ["username"]
  }'
```

### Bug Report Template

```
## Bug Description
<What's happening>

## Steps to Reproduce
1. <step>
2. <step>

## Expected Behavior
<What should happen>

## Actual Behavior
<What actually happens>

## Environment
- OS: <os>
- Version: <version>
```

### Feature Request Template

```
## Feature Description
<What you want>

## Motivation
<Why this would be useful>

## Proposed Solution
<How it could work>

## Alternatives Considered
<Other approaches>
```

## 3. Managing Issues

### Add/Remove Labels

**With gh:**

```bash
gh issue edit 42 --add-label "priority:high,bug"
gh issue edit 42 --remove-label "needs-triage"
```

**With curl:**

```bash
# Add labels
curl -s -X POST \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/issues/42/labels \
  -d '{"labels": ["priority:high", "bug"]}'

# Remove a label
curl -s -X DELETE \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/issues/42/labels/needs-triage

# List available labels in the repo
curl -s \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/labels \
  | python -c "
import sys, json
for l in json.load(sys.stdin):
    print(f\"  {l['name']:30}  {l.get('description', '')}\")"
```

### Assignment

**With gh:**

```bash
gh issue edit 42 --add-assignee username
gh issue edit 42 --add-assignee @me
```

**With curl:**

```bash
curl -s -X POST \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/issues/42/assignees \
  -d '{"assignees": ["username"]}'
```

### Commenting

**With gh:**

```bash
gh issue comment 42 --body "Investigated — root cause is in auth middleware. Working on a fix."
```

**With curl:**

```bash
curl -s -X POST \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/issues/42/comments \
  -d '{"body": "Investigated — root cause is in auth middleware. Working on a fix."}'
```

### Closing and Reopening

1. Read the exact issue body and comments with `gh issue view NUMBER --repo OWNER/REPO --json number,title,state,body,comments,url`, then reconcile its criteria with current owner direction under the SKILL.md core discipline.
2. For artifact-producing work, load `deliver-work` and satisfy its delivery sequence before closure. Follow the repository's canonical-home convention: for wayfinding decisions, put the complete resolution, evidence, implications, alternatives, limits and handoff in the sub-issue body, preserving the original question and identifying superseded requirements. Build a concise completion comment pointing to that body or the task's explicitly chosen artifact home, stating the actual evidence, the owner decision resolving scope, retained limitations, and named links/attachments to the substantive outputs. The full answer may live in accessible linked reports; check the intended reader's delivery surface rather than treating local paths as shared URLs. If earlier comments incorrectly called accepted limitations blockers, explicitly supersede that conclusion rather than leaving contradictory status guidance.
3. Close the intended target with an explicit reason and the comment: `gh issue close NUMBER --repo OWNER/REPO --reason completed --comment BODY` for completed scope; use `--reason 'not planned'` for cancellation.
4. Read the exact target back and verify both its state and the posted comment before reporting closure. For a requested range, parse the returned issue numbers and states programmatically and assert the complete expected set; closing a subset is not completion.

**With gh:**

```bash
gh issue close 42
gh issue close 42 --reason "not planned"
gh issue reopen 42
```

**With curl:**

```bash
# Close
curl -s -X PATCH \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/issues/42 \
  -d '{"state": "closed", "state_reason": "completed"}'

# Reopen
curl -s -X PATCH \
  -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/$OWNER/$REPO/issues/42 \
  -d '{"state": "open"}'
```

### Publishing large canonical resolutions

Use when a canonical body must retain a substantial contract, findings and required review/evidence returns, especially when separate payload or report files are outside scope.

1. **Size before composing.** Inventory mandatory retained text, required complete returns, substantive findings and pending decision sections. Measure the assembled body with `len(body)` and `len(body.encode('utf-8'))`; compare the relevant unit with the destination's confirmed limit. Reserve room for later owner judgment when it must live in that body. Set section allowances once instead of repeatedly writing an oversized resolution and shaving sentences until it fits, because that makes omissions hard to detect.
2. **Remove repetition, not evidence.** Keep each fingerprint/table and substantive conclusion in one authoritative section, then use section pointers. Retain recognizable locations, reasons, alternatives, preservation boundaries and limitations. Preserve original text exactly where required; otherwise preserve its question and intent with only authorized reconciliation. Keep mandatory complete review returns intact. When integrating an immutable research return with review corrections, retain the original evidence and use separately recorded, source-warranted corrections to assemble final prose in memory. Verify each exact replacement against its intended occurrence and inspect surrounding meaning; a unique textual match does not establish a correct repair. Remove demonstrably noncanonical runtime delivery wrappers, such as trailing attachment directives, without dropping research or its final Sources list. Reconcile worker-phase readiness or publication-pending wording in the current final disposition before publishing a completed resolution; preserve immutable returns and historical check records, and leave actual owner-acceptance limits intact. Record coordinator edits separately and rerun affected checks on the complete original-contract-plus-corrected-resolution, because checking the worker snapshot alone does not establish publication readiness. If irreducible required content exceeds the limit, request an authorized alternative rather than silently drop findings, create a prohibited report or move canonical evidence into comments.
3. **Prepare an explicit field-only write.** Prefer `gh issue edit NUMBER --repo OWNER/REPO --body-file PAYLOAD` when payload files are allowed. When they are prohibited, pass the body directly without creating one. For a large body, the exercised stdin route avoids shell quoting and argument-size problems:

   ```python
   payload = json.dumps({'body': body}, ensure_ascii=False)
   subprocess.run(
       ['gh', 'api', '--method', 'PATCH',
        f'repos/{owner_repo}/issues/{number}', '--input', '-'],
       cwd=worktree, input=payload, text=True,
       capture_output=True, check=True,
   )
   ```

   Import `json` and `subprocess`; supply the verified repository, target, worktree and complete body. Send only intended mutable fields; leave labels, assignments and state untouched for a body-only update.
4. **Refresh, write, read back.** Immediately before mutation, fetch the exact current body/discussion and compare with the publication's source version; merge intervening direction first. After mutation, fetch the complete target again and require exact intended-body equality. Compare state, title, labels, assignees, discussion and relevant native blocker/child identities with their pre-write values. Parse complete captured stdout in Python; print compact verification rather than another full response that may be clipped.
5. **Update the map separately.** Add only a concise finding linked to the verified canonical child body. Distinguish executed checks, desktop/local artifact delivery and outstanding owner acceptance; keep acceptance-gated work open. Fresh-read and exact-readback the parent independently, preserving unrelated decisions and native relationships.
6. **Reconcile the completed batch.** Re-fetch native children and the next decision's blockers, compare child identities and declared totals, and assert the exact closed set and remaining open set. Update stale current parent guidance and task-local active/unpublished/pending-review records from verified outcomes, retaining historical evidence; adding new findings alone can leave the map falsely reporting an evidence blocker. Distinguish this state census from repeated semantic verification of earlier resolutions, and report an evidence-ready owner decision as ready—not approved.

### Resolving read-only audit tickets

1. Read the actual ticket, parent map and discussion; establish required coverage, evidence locations and prohibited outputs before dispatching inspection. Delegate independent evidence areas with shared boundaries, then verify consequential returned claims and reconcile disagreements.
2. Record the complete audit in the repository's canonical ticket home: located observations, likely responsibility, alternative explanations, consequences, decision questions, strengths and useful limits. Keep unselected proposals distinct from decisions. When the body is canonical, preserve the original question and publish with `gh issue edit NUMBER --repo OWNER/REPO --body-file PAYLOAD`; use a comment only when that is the selected substantive home. If local payload files are prohibited, pass the complete body/comment directly instead of creating a report file; for a large canonical body use the stdin route under **Publishing large canonical resolutions** rather than a large shell argument.
3. Verify the full intended body with `gh issue view NUMBER --repo OWNER/REPO --json body,state,url`; for a substantive comment, retain its URL and compare its exact body using `gh api repos/OWNER/REPO/issues/comments/COMMENT_ID --jq '{body: .body, html_url: .html_url}'`. Audit completion does not imply corrective implementation or owner acceptance.
4. Follow Closing and Reopening above. When the map requires a linked resolution summary, build it from a fresh parent body and link to the verified canonical home, not automatically to a comment. Re-read immediately before replacing the parent body and reconcile any intervening changes. Read back the complete result and inspect a focused diff against the freshly read parent body: confirm the linked summary and any authorized supersession landed, and that unrelated decisions, evidence, links and scope boundaries remain intact. Investigate every change outside those intended sections; check exact text only for passages the owner explicitly required to remain unchanged. Explicitly supersede an older unresolved entry if retained prose would otherwise contradict the new disposition. Do not rewrite historical sibling tickets to make their narrative status agree with current state.

### Aligning an existing backlog with an approved specification

1. Establish the exact scope of alignment: active implementation/evaluation tickets and the parent map's current execution guidance. Read the full approved spec, its approval record and relevant decision resolutions, then discover the native children and blockers using the complete-map recipe above. Preserve closed research and decision records as historical evidence; do not rewrite them merely to make their earlier prospective wording sound current.
2. Derive requirements from the entire approved specification: scope, responsibilities, workflows, inputs/outputs, failure paths, preservation, exercises, delivery and acceptance—not only its implementation table. Build a located requirement-to-ticket matrix with substantive ticket excerpts and covered/partial/missing/conflict dispositions before proposing additions. Trace each behavior to a permitted authoring home and an actual exercise owner; an integration ticket that tests a behavior but prohibits edits to its owning resource cannot supply implementation ownership. Distinguish genuinely missing work from stale or under-explicit ticket text: clarify an existing owner rather than create a duplicate ticket or reopen a settled decision.
3. Draft focused replacements for all affected active sections: title, What to build, required inputs, permitted homes, acceptance criteria and Blocked by. Preserve the original intent, located evidence, useful criteria and unrelated boundaries. Name the exact local spec path/base and required sections, and link the reachable published specification when one exists. Replace superseded completion gates rather than merely prepend another override. Keep implementation of selected capacity separate from legitimate non-use in an unsuitable collection or project; non-use does not authorize cancelling the selected feature. Keep role-specific access restrictions scoped to their named roles.
4. Reconcile the parent execution summary and resolved-decision checklist only within the owner's authorization. Retain linked decision history, distinguish satisfied approval gates from still-open implementation blockers, and leave unexecuted work unchecked. State recommended ordering for shared-file integration separately from hard dependencies; shared files call for serialized ownership, not invented blocking edges. Preserve issue states, labels, assignees, comments and native relationships unless their change is explicitly authorized.
5. For a substantial batch, assign draft-only workers disjoint ticket sets and exact permitted payload paths inside the authorized project workspace. For a final completeness audit, independently map requirements from the full specification and add cross-ticket seam review rather than ask workers merely to confirm the earlier edits. Resolve reviewer disagreements against located requirements and permitted ownership, not majority vote. Compute matrix totals programmatically; overlapping independent matrices are not a summed count of unique requirements. Have the coordinator review every proposed body against the coverage mapping and spec before any remote write. Check that exercises, failure paths, preservation controls and delivery/acceptance boundaries remain intact; do not turn editing tickets into production-resource changes or execute their work.
6. Immediately before each write, fetch the exact target with `gh issue view NUMBER --repo OWNER/REPO --json number,title,body,state,labels,assignees,comments,url` and compare it with the draft's source version. Merge intervening changes instead of overwriting them. Publish with `gh issue edit NUMBER --repo OWNER/REPO --body-file PAYLOAD`, adding `--title TITLE` only for an intended title change. Read back and compare the entire intended body/title; separately re-read native blockers and the child inventory to verify preserved relationships. Reconcile the expected and verified target sets programmatically, and report published alignment separately from drafts, implementation and owner acceptance.
7. Preserve original audit findings as evidence about the reviewed revision, then publish one authoritative final disposition identifying each correction and its exact verification level. Mark pre-correction recommendations superseded in the final handoff; otherwise reachable reports can still instruct the reader to fix a defect already corrected. Deliver the full matrices and evidence through the authorized surface without treating local files as published repository links.

### Linking Issues to PRs

Issues are automatically closed when a PR merges with the right keywords in the body:

```
Closes #42
Fixes #42
Resolves #42
```

To create a branch from an issue:

**With gh:**

```bash
gh issue develop 42 --checkout
```

**With git (manual equivalent):**

```bash
git checkout main && git pull origin main
git checkout -b fix/issue-42-login-redirect
```

## 4. Issue Triage Workflow

When asked to triage issues:

1. **List untriaged issues:**

```bash
# With gh
gh issue list --label "needs-triage" --state open

# With curl
curl -s \
  -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/$OWNER/$REPO/issues?labels=needs-triage&state=open" \
  | python -c "
import sys, json
for i in json.load(sys.stdin):
    if 'pull_request' not in i:
        print(f\"#{i['number']}  {i['title']}\")"
```

2. **Read and categorize** each issue (view details, understand the bug/feature)

3. **Apply labels and priority** (see Managing Issues above)

4. **Assign** if the owner is clear

5. **Comment with triage notes** if needed

## 5. Bulk Operations

For batch operations, combine API calls with shell scripting:

**With gh:**

```bash
# Close all issues with a specific label
gh issue list --label "wontfix" --json number --jq '.[].number' | \
  xargs -I {} gh issue close {} --reason "not planned"
```

**With curl:**

```bash
# List issue numbers with a label, then close each
curl -s \
  -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/$OWNER/$REPO/issues?labels=wontfix&state=open" \
  | python -c "import sys,json; [print(i['number']) for i in json.load(sys.stdin)]" \
  | while read num; do
    curl -s -X PATCH \
      -H "Authorization: token $GITHUB_TOKEN" \
      https://api.github.com/repos/$OWNER/$REPO/issues/$num \
      -d '{"state": "closed", "state_reason": "not_planned"}'
    echo "Closed #$num"
  done
```

## Native Sub-Issues

Create the parent and children first; retain each displayed `number`, REST `id` and URL as soon as creation is verified. Link a child using its REST database ID:

```bash
gh api repos/OWNER/REPO/issues/PARENT_NUMBER/sub_issues \
  --method POST -F sub_issue_id=CHILD_REST_ID --jq '{id,number}'
gh api repos/OWNER/REPO/issues/PARENT_NUMBER/sub_issues \
  --paginate --jq '[.[] | {id,number,title,state}]'
```

Compare the returned child identities and order with the intended map before dispatch; a parent link in prose is not proof of a native relationship. Persist verified handles incrementally so an interrupted batch can resume without recreating issues.

## Native Blocking Dependencies

When publishing dependent tickets, create blockers first and retain both each issue's displayed `number` and numeric REST `id`. Add the native relationship on the blocked issue:

```bash
gh api repos/OWNER/REPO/issues/BLOCKED_NUMBER/dependencies/blocked_by \
  --method POST -F issue_id=BLOCKER_REST_ID
```

Use the blocker's REST `id`, not its issue number, as `issue_id`. Keep readable blocker references in the issue body too. Verify the native relationship by reading the exact target:

```bash
gh api repos/OWNER/REPO/issues/BLOCKED_NUMBER/dependencies/blocked_by \
  --jq '[.[] | {number,id}]'
```

Compare the returned blocker set against the approved edges, including empty sets on frontier issues. Read back each issue's title, body, labels and state before claiming publication complete. Do not change a historical parent issue merely to publish a new ticket set.

For programmatic `gh api` reads and mutation responses, use `--jq` to select only the fields needed before parsing tool output. Full issue listings and sub-issue mutation responses can include long or nested bodies and exceed the terminal output budget, leaving invalid truncated JSON. A pre-publication duplicate/state sweep typically needs only `number,title,state,updated_at`; paginate when the returned page is full. If response parsing fails after a write, read back the exact target or relationship with a compact projection before retrying, because the remote mutation may already have succeeded. Reconcile existing identities with the intended set and resume only missing writes; never restart a creation batch blindly.

## Quick Reference Table

| Action | gh | curl endpoint |
|--------|-----|--------------|
| List issues | `gh issue list` | `GET /repos/{o}/{r}/issues` |
| View issue | `gh issue view N` | `GET /repos/{o}/{r}/issues/N` |
| Create issue | `gh issue create ...` | `POST /repos/{o}/{r}/issues` |
| Add labels | `gh issue edit N --add-label ...` | `POST /repos/{o}/{r}/issues/N/labels` |
| Assign | `gh issue edit N --add-assignee ...` | `POST /repos/{o}/{r}/issues/N/assignees` |
| Comment | `gh issue comment N --body ...` | `POST /repos/{o}/{r}/issues/N/comments` |
| Close | `gh issue close N` | `PATCH /repos/{o}/{r}/issues/N` |
| Search | `gh issue list --search "..."` | `GET /search/issues?q=...` |
