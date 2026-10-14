---
name: to-spec
description: Turn an idea or discussion into a concrete specification.
version: 0.1.0
author: Balauru, Hermes Agent
license: MIT
platforms: [linux, macos, windows]
disable-model-invocation: true
metadata:
  hermes:
    tags: [specification, synthesis, planning]
    category: productivity
    related_skills: []
---

# To Spec

This skill takes the current conversation context and relevant project understanding and produces a spec. Do NOT interview the user; just synthesize what you already know.

Save specifications under `<project-root>/docs_ai/` by default, then offer publication as a closed issue using `clarify`. Issue-tracker access is needed only if the user accepts that offer. The issue tracker and triage label vocabulary should have been provided to you. If not, tell the user to run /setup-matt-pocock-skills.

## Procedure

### 1. Gather the relevant context

Use the current conversation and read user-supplied material with `read_file` or the appropriate retrieval tool. Use `search_files` to locate relevant local context when necessary. Inspect only what could change the specification; repository exploration applies only to software work that needs it. Use an existing domain glossary and respect recorded decisions where they apply.

Resolve `<project-root>` from the supplied project path or known active workspace root; in a repository, use the repository root rather than a nested working directory. Ask for the destination only if the project root is ambiguous.

**Complete when:** the intent, stated requirements, boundaries, and relevant source material have been identified. If there is no identifiable idea, ask for the missing subject rather than generating a generic specification.

### 2. Separate decisions from gaps

Reconcile the latest user direction with earlier material. Preserve agreed choices; label assumptions, suggestions, and unresolved decisions. Verify consequential factual claims when they are retrievable. Do not invent requirements, values, references, or approvals to make the document look complete.

**Complete when:** each consequential uncertainty is either resolved from available context or recorded with what depends on it. Do not turn gaps into a questionnaire; ask a narrow clarification only if conflicting intent prevents a coherent draft.

### 3. Write the specification

Read `templates/spec.md`, relative to this skill directory, before drafting. Populate its structure with actual content, replacing prompts and removing empty or irrelevant optional entries. Preserve **Problem**, **Key Features**, and **Scope and Boundaries**. Capture every essential requirement without padding the document with an exhaustive user-story list.

The specification's **Workflow** describes how the proposed solution operates or is experienced. Its **Definition of Done** describes observable conditions for the solution to count as complete or established. Include a suitable way to assess each condition; do not claim these conditions have already been met.

For software work, record relevant architecture and testing decisions in **Notes and Open Questions**. Retain agreed test boundaries and relevant existing test examples when known; mark unresolved choices explicitly. Prefer existing verification boundaries and observable behavior over prescribing internal structure. Exact reference paths and agreed output destinations belong where needed; avoid inventing implementation paths or details. A decision-rich prototype excerpt may be included when it is more precise than prose.

**Complete when:** the populated specification explains the solution, its scope, its operation where applicable, and how its essential requirements will be assessed. Known gaps remain explicit.

### 4. Check coherence

Check each essential feature against Definition of Done. Reconcile values and terms across sections, preserve exclusions and constraints, and include important alternatives or failure cases when they affect the stated requirements. Distinguish minimum acceptance from additional quality or longer-term impact.

**Complete when:** every essential requirement has an assessment criterion, no criterion silently expands scope, and unresolved choices are not presented as settled or approved.

### 5. Save the specification; optionally publish a closed issue

Write the complete specification to `<project-root>/docs_ai/<spec-slug>.md` by default, creating `docs_ai/` if needed. Derive a concise, lowercase, hyphen-separated slug from the specification's title or subject. An explicit user-supplied filename or destination overrides this default. Use `write_file` for a new file; read an existing target before updating it with `patch` or `write_file`. Preserve accepted content outside the requested revision. If the target file belongs to a different specification, choose a distinct descriptive slug rather than overwrite it.

After saving the local file, call `clarify` with one question: “The specification is saved at <saved-path>. Also publish it as a closed issue?” Replace <saved-path> with the actual path and supply `choices` as `["Keep it local", "Publish as a closed issue"]`. Ask at this point rather than requiring a publication request beforehand. This delivery choice is separate from the no-interview rule for drafting the specification.

Publish only after an explicit affirmative answer to that question. A local-only choice, skipped answer, cancellation, timeout, or undelivered prompt leaves the saved specification local; do not publish or repeatedly prompt. If `clarify` is unavailable, retain the local file and report that publication confirmation could not be obtained.

If the user accepts, publish the saved specification as a **closed issue**, in addition to the local file. Resolve the intended tracker and project before publishing. Include the full saved specification and identify the issue as a specification record: closure records completion of the specification, not implementation of the proposed solution. Apply only requested or explicitly configured labels; do not add an execution-readiness label automatically.

Use the available tracker tool or CLI through `terminal`. Set the issue's closed state explicitly. If the tracker cannot create a closed issue directly, create it and immediately close that exact new issue. Read back its title, body, and state before claiming publication succeeded. Do not close an unrelated existing issue. If publication or closure fails, retain the local specification and report the actual remote state and issue URL when available.

**Complete when:** the local specification is saved, the post-save publication question has been handled, and any accepted issue copy has been verified closed, or the unmet delivery step is explicitly reported as blocked. Return the local path, any published issue URL, and consequential open questions in conversation. Do not start implementation or ticket creation.

## Pitfalls

- Confusing the skill's authoring procedure with the solution's Workflow.
- Filling gaps with plausible details or carrying superseded decisions forward.
- Turning general variables into mandatory paths, tools, or settings.
- Treating specification completion as solution completion, owner approval, or execution readiness.
- Forcing code, tests, user stories, or publication requirements onto unrelated projects.

## Verification

- The document covers the stated problem, solution, essential features, and boundaries.
- Definition of Done accounts for every essential requirement with an appropriate assessment method.
- Assumptions and open decisions are visible; required references are concrete and checked where accessible.
- Template prompts and irrelevant sections have been removed from the populated output.
- The local specification is saved under `<project-root>/docs_ai/` unless explicitly directed elsewhere.
- Publication was offered through `clarify` after saving the specification; only an explicit affirmative answer authorized a remote write.
- Any accepted issue copy contains the saved specification and is verified closed; publication or closure failures are disclosed separately from local delivery.
- No unrelated project work or publication was performed.
