---
name: grilling
description: Use when grilling plans, ideas, or decisions.
version: 0.1.0
author: balauru, Hermes Agent
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [interview, intent, decisions, exploration]
---

# Grilling

Interview the user relentlessly until you reach a shared understanding of what they actually want and the decisions that follow. Map the conversation as an evolving **design tree** and explore it in dependency-aware **rounds**. Intent discovery stays active throughout; this is neither a fixed questionnaire nor an implementation workflow.

## Why intent comes first

What people ask for and what they actually want are different things. They ask for "a dashboard" because that's what one asks for, not because a dashboard solves their problem. They say "make it faster" without a number to hit.

The cheapest moment to find this gap is before any plan, spec, or code exists. Once you've started building, switching costs are real, and the user will rationalize the wrong thing into a "good enough" thing. The misfit gets locked in.

## When to Use

- The user asks to be grilled, interviewed, or to stress-test a plan, decision, or idea.
- An underspecified goal would require consequential assumptions about who benefits, why it matters, success, constraints, or competing values.
- A stated solution may be a conventional answer to a different underlying need.

For self-contained tasks, factual explanations, or mechanical operations, answer or act directly. Respect an explicit request to skip or end the interview; identify unresolved assumptions rather than treating an interrupted interview as complete.

## Procedure

### 1. Establish the current read

Read the supplied context and inspect relevant accessible sources with `read_file`, `search_files`, `web_search`, or `web_extract`. Reuse established answers; distinguish confirmed user statements from your interpretations and retrieved facts.

For this user's research-backed design interviews, provide concrete source mechanisms before asking questions that depend on them. Read the relevant primary instructions/code, schemas/templates and examples, using earlier investigations and discussions as navigation rather than replacement source evidence. Show actual inputs, ordered construction, intermediate fields, consumer handoffs and alternatives—not abstract feature labels. A complete cross-corpus briefing still requires the requested complete investigation/discussion coverage and verification of consequential source warrants; label narrower briefings honestly. Do not make that entire campaign a prerequisite to resolving basic intent, artifact choice or edit authority.

When the user rejects analysis-first skill/framework work, establish whether the exemplar supplies construction style, actual capabilities or both; clarify which old architecture/interface constraints are reopened and whether the review object is the skills or their generated outputs. Reuse established answers. Once draft authoring is authorized, expose a complete actual skill and required supports early rather than offer another dossier or output-only prototype. This is an inspectable proposal checkpoint, not a bypass of shared-understanding, execution or acceptance gates.

Keep unconfirmed proposal choices visibly unapproved and identify owner-selected branches explicitly. Use `grounded-citations`, **Repository mechanism inspections**, for primary-source fidelity, full-corpus comparison and source/adaptation companions.

State a concise, corrigible hypothesis:

> **Current read:** You want <outcome> for <beneficiary>, because <stated reason or explicitly labeled inference>.
> **Still unclear:** <the unresolved questions that could change this interpretation>.

When outcome, beneficiary, or reason is not established, mark it unspecified; any proposed interpretation must remain explicitly tentative and grounded. Describe uncertainty through missing information. If the user names an artifact or solution, treat it as a candidate means rather than silently equating it with the goal. If intent is already concrete, say so and start from the unresolved decisions.

**Complete when:** the current interpretation is visible, its consequential assumptions are exposed, and existing answers have not been re-opened without a reason.

### 2. Build the evolving design tree

Every decision branches into the decisions that hang off it. Track discovered questions, their prerequisites, answers, and whether they are **open**, **settled**, or **blocked** in the working conversation context. This is a thinking aid, not a new file, task board, or coordination system.

Explore the outcome, beneficiary, motivation, success, constraints, competing values, and exclusions as they arise. Follow consequences, alternatives, edge cases, failure modes, reversibility, and tensions between answers. Let surprising answers introduce new branches or replace the original framing. Do not use the opening topic map as a ceiling on exploration.

The **frontier** is every open question whose prerequisites are settled. A question whose answer depends on another open question belongs to a later round. A pending fact lookup is also an unsettled prerequisite.

**Complete when:** the currently known branches and dependencies are mapped, and the frontier contains only questions that can be answered without guessing at unsettled prerequisites.

### 3. Ask the frontier in rounds

Cover the whole frontier in a round. When it is wide, use manageable batches and retain the remaining questions for subsequent batches; smaller batches change pacing, not exploration depth. Wait for answers and recompute dependencies before the next batch. Ask a foundational question alone when its answer could invalidate the other questions you would ask.

Number each question and attach your best grounded tentative answer with concise reasoning. If neither an evidence-grounded hypothesis nor a goal-supported recommendation is available, name the missing grounding and ask neutrally instead of manufacturing an answer. State a shared evidence gap once per batch rather than repeating empty hypothesis labels.

When offering a tentative answer:

- **Hypothesis** predicts the user's intent or preference. Ground it in their statements or label the inference; invite correction.
- **Recommendation** advises a choice. Explain why it serves the stated goal and name the tradeoff when consequential.
- When both are needed, label them separately. Advice is not evidence of what the user wants. Do not invent a preference or a deliberately wrong guess to provoke a reaction.

Use `clarify` when available: put independent questions in one form, within its current question and choice limits. For intent questions, use an open-ended field with the labeled hypothesis in the question text. For decisions with concrete alternatives, put the recommended option first in `choices`; options belong in `choices`, not in the question text. Include concise reasoning in the question text. Skipped or unanswered fields remain open.

If `clarify` is unavailable, use this conversational format and wait for the user's reply:

> **Q1 — <title>:** <one focused question>
> **Hypothesis or recommendation:** <tentative answer and reason; label which it is>
>
> **Q2 — <title>:** <another independent question>
> **Hypothesis or recommendation:** <tentative answer and reason; label which it is>

Finding **facts** is your job, never the user's. Look up what you can retrieve; use `delegate_task` for substantial independent exploration. Give a researcher the exact sources or discovery target, the factual question, and a read-only boundary. While research runs, ask unrelated frontier questions. Wait for its result before using it to unblock dependent decisions; if delivery requires ending the turn, end it. Treat reports as evidence to check, not as user decisions. If a fact is genuinely inaccessible, ask only for the missing information or access the user can provide.

**Complete when:** the ready questions have been presented in independent batches, hypotheses and advice are distinguishable, and answers or missing responses are available to update the tree.

### 4. Probe intent and update the tree

Listen for **want versus should want**: "scalable," "clean," "modern," "the standard approach," or "I should probably" without a concrete need. These are prompts to investigate, not proof that the answer is insincere. Ask for the situation, consequence, or tradeoff that makes the quality necessary. When useful, ask:

> If you didn't have to justify this to anyone, what would you actually want?

For each answer, test whether it changes the goal, contradicts another answer, or creates downstream decisions. Retain valid answers, re-open invalidated ones, and recompute the frontier. Briefly show the changed understanding rather than replaying the whole interview.

When a persistent briefing or decision record is already authorized, synchronize its current status and affected row labels with each settled answer before asking dependent questions. Distinguish accepted conceptual dispositions, final contract confirmation, conditional prototype permission and acceptance of actual outputs; a blanket “unapproved” label can become false while implementation remains unauthorized. Preserve historical review records as evidence about their inspected version rather than rewriting them to imply later approval or execution.

The user owns decisions and may explicitly delegate them. "Choose for me" is legitimate delegation, not failed understanding: decide within known goals and boundaries, record the rationale, and ask only when missing boundaries would change the choice or risk. Delegation of one choice does not imply approval of the entire understanding or authorization for downstream work.

If rounds recycle without clarifying anything, identify the unresolved foundation and reframe the question, contrast concrete situations, or ask whether to pause. Reframing changes the route, not the exhaustive stopping rule. A skipped answer or deferred prerequisite stays unresolved. Distinguish it from an optional addition the owner explicitly defers out of the current scope: record that disposition, its tradeoff and any re-entry condition as settled for this interview, because requiring its future design would reopen an accepted boundary.

**Complete when:** each answer has been integrated, newly exposed branches are recorded, and invalidated decisions and remaining uncertainties are visible. Return to step 3 whenever a frontier remains.

### 5. Exhaust the tree and confirm shared understanding

**Exploration is exhausted when every branch of the design tree has been visited and no open or blocked question remains, nothing left silently assumed. The session is done after the concrete restatement is confirmed.** An empty ready frontier with blocked or unanswered nodes is not completion. Check those nodes and newly exposed consequences before declaring convergence. Alternatives count as visited when they have been examined and the reason for choosing or rejecting them is explicit, not merely because they were listed.

The user can change the topic boundaries or end the interview. Record that choice and the affected branches; do not quietly narrow the exploration yourself. If they stop with unresolved branches, provide a partial understanding and name what remains open rather than claiming the exhaustive session is complete.

Once the tree is exhausted, restate the understanding in the user's language:

- **Outcome:** what they actually want, distinct from the proposed means.
- **Beneficiary:** who this serves.
- **Why:** their motivation; why now when timing matters.
- **Success:** observable evidence that it worked.
- **Constraints and priorities:** binding limits and chosen tradeoffs.
- **Out of scope:** explicit exclusions and rejected directions.
- **Decisions:** the settled choices, including choices explicitly delegated to you.

Keep the intent summary tight; include enough decision detail to preserve every settled branch without burying it. For instruction/resource selection, name the exact existing homes, changed responsibilities and required handoff inputs; distinguish candidate resources from existing inputs and state which roles a boundary applies to. Separate confirmation of those boundaries from authorization to implement them, because agreement with a feature label does not settle ownership or delivery. Mark genuinely absent motivations or constraints as unspecified, not invented. Ask the user to confirm or correct the concrete restatement. If a correction opens a branch, resume exploration.

Accept clear contextual approval, including "sounds good" or "let's go" when it plainly approves this restatement; no magic word is required. Clarify genuinely ambiguous approval. Silence is not confirmation, and agreement with a recommendation is not blanket approval of the restatement.

**Complete when:** every branch has been visited, no question is silently unanswered or blocked, and the user has confirmed the concrete shared understanding.

### 6. Hand back the confirmed understanding

Deliver the confirmed intent and decisions. Keep them in conversation unless the user asks for a persistent document; confirmation alone does not authorize saving one. Separate any remaining external limitations from settled choices.

Proceed to a spec, plan, or implementation only if that downstream action is already authorized and the shared-understanding gate has passed. Otherwise hand control back with an appropriate next step, without starting it. Approval can permit already-authorized downstream work in the same turn; there is no compulsory extra-turn pause.

**Complete when:** the confirmed understanding is delivered and any continuation stays within the user's actual authorization.

## Pitfalls

- A fast batch is not a shallow interview: carry every unasked branch forward.
- A familiar solution is not yet a demonstrated need: keep testing the means against the outcome.
- A generic quality can be a real requirement: establish its concrete stakes rather than dismissing it as posturing.
- A recommendation or inferred motive is not a confirmed user preference: retain the label until supported.
- A tidy restatement is not convergence: inspect the tree for unanswered branches before seeking approval.

## Verification

- [ ] The current intent hypothesis and consequential uncertainties were made visible.
- [ ] Retrieved facts, user statements, interpretations, and recommendations remained distinguishable.
- [ ] Questions were dependency-aware; batching left no sibling or downstream branch forgotten.
- [ ] Conventional answers and unsupported qualities were probed when present; changed intent reshaped the tree.
- [ ] Every branch was visited and no open or blocked question was hidden at completion; an early stop was labeled partial.
- [ ] Outcome, beneficiary, motivation, success, constraints, exclusions, and decisions were restated and contextually confirmed.
- [ ] Delegated decisions stayed within their boundaries; persistence and downstream actions respected actual authorization.
