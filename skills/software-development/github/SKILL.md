---
name: github
description: "GitHub via gh CLI: PRs, issues, reviews, repos, auth."
version: 2.0.0
author: Ben Barclay (benbarclay), Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [github, gh, git, pull-requests, issues, code-review, repos, auth, ci]
    category: software-development
    related_skills: [codebase-inspection, requesting-code-review]
---

# GitHub

Work GitHub end to end with the `gh` CLI (REST fallback where noted): auth,
issues, the PR lifecycle, issue-to-PR delivery, code review, and repo
management. This skill consolidates six former skills; each workflow lives
complete in its reference file — ALWAYS read the matching reference before
starting that workflow, the body below only routes.

## Routing

| Task | Read first |
|---|---|
| Auth broken / new machine / token or SSH setup / gh login | `references/auth.md` |
| Create, triage, align a backlog with an approved spec, label, assign, close issues | `references/issues.md` |
| Branch, commit, open PR, watch CI, merge | `references/pr-workflow.md` |
| Carry an ISSUE to a verified PR (full delivery loop) | `references/issue-to-pr.md` |
| Review someone's PR: diffs, inline comments, verdict | `references/code-review.md` |
| Clone/create/fork repos, remotes, releases | `references/repo-management.md` |

Supporting assets: `scripts/gh-env.sh` + `scripts/git-credential-token.py`
(auth helpers), `templates/` (PR bodies, bug report, feature request),
`references/ci-troubleshooting.md`, `references/conventional-commits.md`,
`references/github-api-cheatsheet.md`, `references/review-output-template.md`.

## Core discipline (applies to every workflow)

- Preflight once per session: `gh auth status` — if it fails, go to
  `references/auth.md` before anything else.
- Prefer `gh` over raw REST; drop to `gh api` only for endpoints the
  porcelain lacks (the cheatsheet lists them).
- Never report CI green without checking `gh pr checks` yourself; never
  claim merged without verifying `state,mergedAt`.
- Read full context before interviewing, planning or writing: `gh issue view
  --comments` / `gh pr view --comments` — decisions live in threads, not titles.
  For a child in an issue map, read the parent and discover its actual sub-issues;
  gather relevant sibling bodies and complete discussions, including closed
  research/audits and downstream scopes, before asking the owner to decide.
  When the owner requests the whole map, read every child, not just direct
  blockers. Use the complete-map recipe in `references/issues.md` before the
  interview; parent summaries and clipped tool output are not full context.
  Read owner responses across the whole form or conversation; one field must
  not erase revision intent or qualifications supplied in another.
- Reconcile an owner-reported commit at artifact granularity before describing
  work as uncommitted: inspect `git status --short`, `git branch --show-current`
  and `git log -5 --oneline`, then `git show --stat <resolved-commit>` for the
  referenced change; search further history if that commit is not in the preview.
  Distinguish the existing committed contribution from files generated or edited
  afterward. A dirty tree does not disprove an earlier commit, and an existing
  commit does not establish that later deliverables are included or published.
- Classify the requested outcome from the actual ticket before acting: an
  exploratory audit or decision input is not implementation authorization.
  Complete its evidence deliverable without manufacturing production edits.
- Interpret a request to close remaining subissues against each child's actual
  completion conditions. Advance independently executable work, but retain real
  owner-decision gates: a bulk closure request does not supply contract choices,
  prototype feedback or specification acceptance. Name the outstanding decisions
  without asking the owner to repeat settled requirements.
- Apply explicit owner acceptance and scope changes when deciding completion.
  Record executed evidence and accepted limitations separately; do not keep a
  ticket open solely for an exercise the owner says is unnecessary or invent
  another edit to satisfy its earlier checklist. Acceptance does not establish
  that an unexecuted check happened. Read live issue state separately from
  narrative status in older comments; a closed issue alone proves neither
  owner acceptance nor successful historical execution.
- When aligning an existing backlog with approved decisions, reconcile the active
  titles, scope, acceptance criteria and handoffs themselves; a supersession note
  above contradictory instructions leaves implementers with competing contracts.
  Preserve closed research/decision history, and distinguish implementation of a
  selected capability from conditional use of that capability. Follow the backlog
  alignment recipe in `references/issues.md`; alignment is not implementation or
  output acceptance. For completeness checks, derive obligations from the entire
  approved specification, not its ticket table. Require a permitted implementation
  owner as well as an exercise owner; testing a behavior does not assign its authoring.
- Before replacing an issue or PR body, size the complete intended publication
  against the destination's confirmed limit, retaining original intent and required
  evidence plus room for pending decisions; append-only resolutions can leave no
  room to record acceptance. Use the large-resolution recipe in `references/issues.md`
  when needed. Freshly read the exact target and compare it with the replacement's
  source version. If it changed, merge into the latest body rather than overwrite
  concurrent work; read back and compare the full intended body after the write.
- Sweep for duplicates before creating anything:
  `gh pr list --search` / `gh issue list --search`.

## Verification

- The workflow's own reference file defines done for that task.
- Cross-cutting: every claim about remote state (CI, merge, release,
  issue state) is backed by a fresh `gh` read, never memory.
