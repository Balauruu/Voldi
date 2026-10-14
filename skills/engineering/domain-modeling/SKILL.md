---
name: domain-modeling
description: "Sharpen domain language, glossaries, and decisions."
version: 0.1.0
author: "Matt Pocock (origin), Balauruu, Hermes Agent"
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [domain-modeling, glossary, terminology, adr]
    related_skills: []
---

# Domain Modeling

Actively sharpen the project's domain model while designing: challenge language, probe boundaries with concrete scenarios, and capture confirmed meanings promptly. Reading a glossary for vocabulary alone is not this discipline. Adapted from Matt Pocock's original skill with local revisions; see [LICENSE](LICENSE).

## When to Use

- Use during design or terminology discussions, when changing `GLOSSARY.md`, or when recording or editing an ADR.
- Do not use merely to consume vocabulary, implement features, or record routine choices. Automatic invocation remains eligible; no explicit slash command is required.

## Inputs and authority

- **Required to begin:** a domain question, terms, relationship, or decision to sharpen, and the user's statements about it. If absent, ask what concept or decision needs work.
- **Required for repository work:** the project root or an unambiguous already-known workspace, plus the allowed edit scope. Discover an existing root from context; ask if ambiguous. No project access is needed for discussion, but mark file/code checks unavailable. An absent permission is not permission: discuss or present drafts without writes.
- **Discover before edits:** root `GLOSSARY-MAP.md`, the applicable `GLOSSARY.md`, and relevant existing ADRs. Missing glossary/ADR directories are allowed; create them only at their production steps. Optional code access changes whether behavior claims can be checked, not whether definitions may be discussed.
- **Authority:** the map determines context homes; each context glossary governs its own language. Preserve the exact local meaning, capitalization, and existing unrelated entries. Another context, a generic dictionary, or code does not silently override that meaning. Explicit human resolution may change it; surface the conflict first.
- **Unknown/conflicting inputs:** ask if the answer changes context selection, a term's definition, a decision's status, or a write target. Keep other unknowns in the conversation as `Unknown: ...`. A suggestion, invented scenario, or unanswered question is not a confirmed term or decision. If authorities disagree, quote both and ask which governs; do not resolve by majority or by guessing.

## Procedure

### 1. Locate the context and read its language

Use `search_files` and `read_file` within the known project. If root `GLOSSARY-MAP.md` exists, read it and follow its paths even if a root glossary also exists. Infer the topic's context; ask if unclear or cross-context ownership is disputed. Otherwise use root `GLOSSARY.md`, creating nothing yet if absent. Context-specific ADRs live at that context's `docs/adr/`; system-wide ADRs live at root `docs/adr/`. Read relevant decisions before proposing changes.

[GLOSSARY-FORMAT.md](./GLOSSARY-FORMAT.md) gives the existing map format and routing examples; it points back here for the authoritative glossary template. Do not introduce a map, extra glossary, or new documentation convention merely to run this skill.

**Done when:** the context and applicable authority are known, or the blocking question is asked without speculative writes.

### 2. Challenge language and test boundaries

Immediately call out conflicts: “Your glossary defines Cancellation as withdrawal of the whole Order, but you seem to mean one line. Which is it?” For fuzzy or overloaded words, propose a precise canonical term: “By account, do you mean Customer or User? Those are different concepts.” Be opinionated about terminology, not presumptive about approval.

When relationships are discussed, use a fitting supplied concrete boundary scenario or invent one and label it hypothetical. For example: “An Order has two lines; one ships, and the customer withdraws the other. Is that a Cancellation?” Or: “One person buys for two organizations; is that one Customer or two?” Ask which concepts and boundaries apply; retain the answers, not the invented events as project facts. Probe the boundary under discussion rather than generating an unrelated checklist.

**Done when:** each current ambiguity has a confirmation or a focused open question, and a discussed relationship has been stress-tested with a concrete scenario.

### 3. Compare behavior claims with code, when possible

When the user states how something works and code is accessible, use `search_files` and `read_file` to inspect the relevant behavior and tests. Cite the actual file/symbol and state the mismatch: “`cancelOrder` cancels all lines, but you said partial cancellation is supported. Is the model changing, or is this implementation wrong?” Neither code nor the claim automatically wins. If unavailable or inconclusive, mark `Not checked` and explain why. Do not invent paths, code results, or implementation changes; glossary-only terminology discussions need no unrelated code audit.

**Done when:** relevant behavioral claims have an evidenced match/conflict or an explicit check limitation.

### 4. Capture each resolved term immediately

As soon as a term is confirmed, update the applicable `GLOSSARY.md` with `patch` (or `write_file` for its first creation) if those file edits are authorized. Do not wait until the end of the session or until all other questions are resolved. Read an existing file before editing and preserve unaffected entries. Without edit permission, present the confirmed entry promptly as an unapplied draft; do not claim it was saved. Do not create a file for proposals alone.

Create `GLOSSARY.md` with these sections, or insert the confirmed entry into its existing structure:

```md
# [Confirmed context name, or Glossary if the name is not supplied but the target is settled]

[One or two sentences grounded in supplied context facts; omit if unsupported.]

## Language

**[Confirmed canonical term]**:
[One or two sentences defining what the concept IS in this project.]
_Avoid_: [Confirmed competing words for this same concept; omit this line if none.]
```

Definitions are one or two sentences, context-specific, and opinionated. List competing synonyms under `_Avoid_`, not distinct concepts or unconfirmed aliases. Group entries under natural subheadings only when useful; a flat list is fine. The glossary is **totally devoid of implementation details**: no programming concepts, database choices, functions, code paths, specs, open questions, or decision rationale. Define what it is, not a procedure for what it does. Derive introductory prose only from supplied facts; if unsupported, omit it and report the gap in the handoff. Missing introductory wording must not delay capture of a confirmed term once context/target and write authority are settled. Unresolved context ownership or destination remains a genuine write blocker; deliver the confirmed entry promptly as unapplied while asking the focused question.

**Done when:** the confirmed term is saved in the correct authorized glossary or clearly delivered as unapplied; proposals remain outside it.

### 5. Offer and produce ADRs only conditionally

For an explicitly requested **maintenance edit to an existing ADR**, preserve its identity/rationale and make only authorized factual, formatting or evidenced status corrections. Do not require retrospective proof of the original decision's reversal cost, surprise and tradeoff; do not invent missing history. Ask about unsupported substantive changes. A new choice or replacement decision record uses the new-record gate below, even if an older ADR is being superseded.

Only offer an ADR when evidence supports **all three**:

1. **Hard to reverse:** a meaningful cost to changing the choice later.
2. **Surprising without context:** a future reader would wonder why this path was taken.
3. **A real trade-off:** genuine alternatives were compared and a choice selected for specific reasons.

If any is missing or unknown, skip the offer and file. A request for an ADR is not evidence of these criteria; explain missing criteria rather than inventing a trade-off. Ordinary term confirmation does not imply an ADR. Qualifying examples are in [ADR-FORMAT.md](./ADR-FORMAT.md).

When all qualify, offer briefly with the reasons. **Create an ADR only when requested or when ADR creation is explicitly within the authorized scope.** General glossary edit permission is not ADR permission. If the choice remains unresolved, ask; do not pick it yourself to make the gate pass. An explicitly requested draft of a supplied, reasoned proposal may be produced as `proposed`, not as an accepted decision.

For a new authorized ADR, scan the target `docs/adr/` with `search_files`; use `terminal` or `execute_code` to compute the highest existing number plus one (start at `0001` if empty). Use `NNNN-slug.md`, e.g. `0001-event-sourced-orders.md`; do not overwrite an existing number. Create the directory lazily only for the first authorized qualifying ADR. Edit an existing requested ADR in place rather than allocating a duplicate.

Create the ADR with this minimal template:

```md
# [Short title of the recorded decision or explicitly labeled proposal]

[One to three sentences stating the context, the confirmed choice (or proposed choice), and the reasons for it, including the real trade-off.]
```

A single paragraph is enough. Only add sections when valuable:

- YAML `status: proposed | accepted | deprecated | superseded by ADR-NNNN` when status needs to be tracked. For any unaccepted draft, use `status: proposed` and proposed language in the paragraph; never imply owner approval.
- `## Considered Options` for rejected alternatives worth remembering.
- `## Consequences` for non-obvious downstream effects.

**Done when:** the requested qualifying record is saved/drafted with supported status and reasons, or the ADR is explicitly skipped/only offered. No unasked ADR file is created.

### 6. Verify and hand off the result

Read back authorized project edits and compare the exact target with the confirmed wording and intended scope. Report any failed write as unsaved; do not let a draft stand in for persistence. Return this compact conversational contract (not a new mandatory file); use `None` for empty sets:

```md
Context: [Context and authoritative glossary path, or discussion-only/unknown.]
Resolved: [Confirmed terms or decisions, with their local meanings.]
Proposed / unknown: [Unaccepted suggestions and open questions, clearly separated from resolutions.]
Challenges / scenarios: [Conflicts surfaced and hypothetical boundary checks with any confirmed answers.]
Code comparison: [Actual evidence and result, or Not checked with reason.]
Artifacts / ADR: [Exact saved paths or unapplied drafts; ADR saved, proposed, offered, or skipped with reason.]
```

**Done when:** saved versus proposed versus blocked work is distinguishable, no invented decision appears, and every claimed write is checked.

## Complete small examples (illustrative, not project edits)

### Confirm one term while another remains proposed

**Complete input:** Known single-context project `shop`; no map, glossary, or ADR directory exists. User authorizes glossary edits only: “Ordering receives and tracks customer orders. Order means a customer's request for goods; avoid Purchase. Cancellation means withdrawal of the whole Order. For withdrawing only an unshipped line, suggest a term but don't record it yet.” No code or behavioral claim is supplied. In the edge scenario with two lines and one shipped, the user confirms withdrawing only the remaining line is not Cancellation; the agent suggests Line Withdrawal, not yet accepted.

**Complete output — `shop/GLOSSARY.md`:**

```md
# Ordering

Ordering receives and tracks customer orders.

## Language

**Order**:
A customer's request for goods.
_Avoid_: Purchase

**Cancellation**:
Withdrawal of the whole Order.
```

**Complete handoff after authorized writes and readback:**

```md
Context: Ordering; shop/GLOSSARY.md (single context).
Resolved: Order = a customer's request for goods; avoid Purchase. Cancellation = withdrawal of the whole Order.
Proposed / unknown: Line Withdrawal is proposed for withdrawing an unshipped line; confirmation is still needed.
Challenges / scenarios: Hypothetical two-line Order with one shipped; user confirmed withdrawing only the remaining line is not Cancellation.
Code comparison: Not checked; no code or behavior claim supplied.
Artifacts / ADR: shop/GLOSSARY.md saved and checked; each confirmed entry captured promptly. ADR skipped: no qualifying hard-to-reverse, surprising trade-off. No ADR directory created.
```

### A requested proposed ADR is not approval

**Complete input:** Known project `commerce`; `GLOSSARY-MAP.md` routes Ordering to `src/ordering/GLOSSARY.md` and Billing to `src/billing/GLOSSARY.md`. Both glossaries already exist and are unchanged. User states a system-wide proposal: “After comparing synchronous calls and events, we selected events as our proposal. The partner requires Ordering to accept orders during Billing outages. Replacing durable event contracts later requires a costly coordinated migration; readers would otherwise expect a synchronous billing check. Events preserve availability but delay billing. The owner has not accepted this proposal. Draft a proposed ADR in the root docs/adr only; don't change glossaries.” No root ADR directory exists; no implemented behavior is claimed.

**Complete output — `commerce/docs/adr/0001-ordering-billing-events.md`:**

```md
---
status: proposed
---
# Proposed event-based Ordering–Billing integration

The partner requires Ordering to accept orders during Billing outages, where readers might otherwise expect a synchronous billing check. We propose events rather than synchronous calls to preserve availability at the cost of delayed billing; replacing durable event contracts later would require a costly coordinated migration. Owner acceptance is pending.
```

**Complete handoff after authorized draft write and readback:**

```md
Context: System-wide integration; commerce/GLOSSARY-MAP.md routes Ordering and Billing to separate authoritative glossaries.
Resolved: No new terms or accepted decisions.
Proposed / unknown: Events over synchronous calls; owner acceptance pending.
Challenges / scenarios: Supplied Billing-outage scenario tests integration availability; no new terminology proposed.
Code comparison: Not checked; no implemented behavior claimed.
Artifacts / ADR: commerce/docs/adr/0001-ordering-billing-events.md saved as proposed and checked. All three ADR criteria are supplied; drafting explicitly requested. No glossary changes or accepted status implied.
```

## Pitfalls

- Do not rewrite local language to match generic domain-driven-design terminology; challenge and obtain resolution first.
- Hypothetical cases expose ambiguity, not facts or human approvals. Proposed terms never belong in the glossary.
- Equal spelling across contexts does not establish equal meaning; the map routes authority, it does not merge definitions.
- Code contradictions belong in the discussion/verification result, not as implementation detail in a glossary.
- Do not batch confirmed terms while waiting on unrelated unknowns; do not create empty files or documentation scaffolding.
- A noteworthy choice alone is insufficient for an ADR; the three-part gate and separate write permission both apply.

## Verification

- Every affected term retains its exact confirmed local meaning; tight definitions contain no implementation details or general programming concepts.
- Unresolved terminology, decisions, and conflicting authority remain explicit questions/proposals, not silent resolutions.
- The map, if present, governed context selection; no new root glossary bypassed it. ADR scope and numbering are correct.
- Concrete scenarios were labeled hypothetical; behavioral code comparisons are evidenced or explicitly limited.
- Confirmed terms were captured promptly where authorized; lazy creation and unrelated entries were preserved.
- Every new substantive ADR/choice satisfies all three criteria and creation permission; authorized maintenance preserves existing rationale without retrospective requalification. Proposal status never implies acceptance.
- The handoff identifies exact saved/draft paths, remaining unknowns, verification, and any skipped conditional output.
