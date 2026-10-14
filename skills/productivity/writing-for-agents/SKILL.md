---
name: writing-for-agents
description: "Write agent-facing documents with clear task contracts."
version: 1.0.0
author: Matt Pocock, Balauru, Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [writing, instructions, handoffs, navigation]
    related_skills: []
---

# Writing for Agents

Turn a task description into a document an agent can use without the author's conversation: a prompt, handoff, procedure, stage contract, reference guide, or navigation index. Improve predictability by making inputs, actions, outputs, and completion conditions explicit. This procedure needs no external service; retrieval and independent exercises depend on the requested task.

## When to Use

- Draft or revise instructions for a fresh agent or isolated worker.
- Review an agent-facing document for ambiguity, missing inputs, conflicting authority, or unusable pointers.
- Create a project navigation index or a reusable fillable document template.
- Construct or audit a source-derived writing guide.

Scope is ordinary documents and task handoffs, not reusable-capability packaging or automatically loaded project-rule files. Writing instructions does not authorize executing their task, publishing them, or building workflow machinery.

## Inputs

- **Task and intended reader:** what the consuming agent must accomplish, and which role or phase the document governs. Retrieve these from the request or supplied material; ask if they cannot be identified.
- **Source material:** supplied requirements, current document for a revision, examples, and governing decisions. Retrieve referenced sources before using them. Ask for inaccessible material that is necessary; otherwise label the affected point unverified.
- **Scope and authority:** allowed edits, protected material, owner overrides, and decisions reserved for a human. Apply an override only to the roles, phases, and inputs it names. Ask about an unresolved consequential conflict rather than invent precedence.
- **Delivery:** requested filename, destination, or conversation output. Resolve relative paths against the declared project root or containing document. Return an inline draft when no file destination is requested or implied; ask only if a required save location remains ambiguous.
- **Optional controls:** an existing template, audience/medium, factual boundaries, permitted tools, review limits, or testing request. Preserve supplied controls; their absence is not permission to invent a pipeline, default voice, paired-review stage, or approval gate.

## Procedure

### 1. Establish the document's job

Read the request and relevant source material with `read_file` or the appropriate retrieval tool. Use `search_files` to resolve named local resources. Identify the consumer, task, existing inputs, outputs to create, and edit boundary.

Separate recorded requirements from examples, proposals, and unresolved choices. For revisions, read the whole document before changing a passage so a local repair does not contradict another section. Keep governing project policies in their existing home rather than copying one project's layout into general guidance.

When the user supplies a construction exemplar, inspect its actual procedure, branches, templates and checks before choosing a document shape. Establish whether it supplies construction style, capabilities or both. For this user's procedure redesigns, make the actual instructions and necessary supports the primary review object; generated outputs and analysis are supporting evidence, not substitutes. Carry concrete operations into a usable candidate before expanding the analysis campaign, because successive summaries can erase the mechanisms the user wanted to reuse.

**Done when:** the task, consumer, permitted scope, and controlling requirements are identifiable; blocking gaps are surfaced.

### 2. Map requirements to operations

For each material requirement, identify the input it uses, action it requires, output it affects, and check that establishes completion. Keep this working map proportional to the task; it need not become another delivered document.

Replace abstract direction with the actual operation: extract decisions, compare specified options, classify records, trace a source claim, or populate named fields. Define when missing information blocks work, remains explicitly unknown, or makes a conditional section inapplicable. Preserve the distinction between fact and interpretation. Keep precise missing-input and unsupported-required-job blockers reportable alongside genuinely blocking owner choices; preserve unaffected work and let compatible differences coexist conditionally. Do not limit escalation to contract incompatibilities, because that wording can suppress legitimate construction blockers; do not turn routine differences into approval gates.

For a reusable construction guide, specify how the consumer selects and builds a fitting treatment: prerequisites, ordered operations, alternatives, a bounded worked transformation, and a quality check. A list of lenses or style adjectives does not supply a construction method.

**Done when:** every requirement has an action, output element, or constraint; suggestions and unresolved decisions remain distinguishable.

### 3. Choose the information hierarchy

Put actions common to every run in the main procedure. Keep a concept's definition, rule, and caveat together. Disclose branch-specific detail through a pointer naming the exact resource, relevant section, what it supplies, and when to read it.

Keep one authoritative home per responsibility: a navigation index routes, a stage contract declares local inputs and outputs, and a procedure explains the work. Link to shared guidance instead of restating it. Resolve environmental facts from authoritative configuration or commands when needed; document the convention or gotcha the environment cannot explain.

Prefer concrete positive actions and stable terminology. A compact label helps only if its action is clear; “tight” or “thorough” cannot replace a completion condition. Sharpen a vague condition before splitting a sequence. Use separate contexts only when a genuine phase or isolation boundary calls for them.

Conditional references, relative to this document's directory:
- **Navigation:** before creating an index, assessing index/contract overlap, or changing placement guidance, read [references/project-navigation.md](references/project-navigation.md), especially the recipe for the requested operation.
- **Source-derived writing:** before auditing a reference-informed guide or designing a writing trial, read [references/reference-informed-writing.md](references/reference-informed-writing.md), especially **Procedure**, **Evidence boundaries**, and the applicable verification recipe. Its specialized exercise controls apply only when the current task requests them.

**Done when:** the main path is executable, optional branches have triggers, and no responsibility has competing authoritative copies.

### 4. Construct the document

Choose the construction shape that fits the requested procedure and any supplied exemplar. Preserve meaningful phase order, operating modes, native field groups and template-to-consumer relationships; do not force every source-derived procedure into generic purpose/selection/application headings. Expanded headings are not an improved method unless they teach the actual decisions and construction.

Use the Agent-Facing Document below as a fallback when no more fitting structure is supplied, not as a mandatory section schema. Replace guidance placeholders with task-specific content. Reference guides and navigation indexes use their purpose-specific structure rather than a fabricated execution sequence.

```markdown
# [Name the task the consuming agent will perform]

## Purpose and scope
[State the result to achieve, intended consumer, and assignment boundaries.]

## Inputs
[Name each actual input and how to obtain it. Distinguish existing inputs from
outputs to create. For each missing input, state whether to ask, record an
unknown, or omit a conditional section. Give the base for relative paths.]

## Procedure
1. [Use a named input to perform a concrete operation and retain its result.]
   Done when: [State the observable condition that completes this operation.]
2. [Use the previous result to construct the promised output. Include further
   task-specific operations in execution order when needed.]
   Done when: [Account for every required output element.]

## Output
[Name each deliverable and its destination. Include its complete fillable
structure, field meanings, conditional sections, and missing-value rules.
For a lookup or status result, define the direct return instead.]

## Checks and stopping
[Specify coverage, factual and scope checks, and the actual stopping condition.
Name decisions requiring the owner and what remains blocked until they answer.]
```

Place a deliverable's template beside the operation populating it when that improves readability. Keep authoring instructions separate from the domain deliverable: an idea brief's Workflow describes the idea's operation, not the agent's drafting process. For fillable templates, use prompting placeholders and domain-neutral guidance; let the future subject determine its variables and criteria.

Include a complete illustrative input and filled output when teaching a reusable transformation and an example resolves a real ambiguity. Declare invented premises and test-only provenance. Keep the example within those premises; a repair or affirmative example can invent facts as readily as bad prose. Preserve an existing required example rather than replacing it with an excerpt.

For an audit-only request, return located findings: the actual passage, violated requirement or ambiguity, consequence, and proposed correction. Separate defects from optional improvements and unverified claims. Do not edit the target unless authorized.

**Done when:** every promised result has a usable structure or defined return, every step contributes to it, and the consumer needs no hidden author context.

### 5. Inspect the revision and references

Read the draft against the requirement map. For edits, inspect a focused diff and surrounding prose using `patch` output or `terminal` for Git diffs when applicable. Check meaning, attribution, constraints, conditional behavior, and dependencies—not merely file existence or valid formatting.

Resolve local links from the containing file; `#heading` refers to that file itself. Check actual headings and load-bearing targets. Distinguish missing existing inputs from planned outputs, and local-only files from published resources. Report inaccessible references instead of asserting they work.

Prune repeated meanings, stale task history, and lines with no identifiable effect on the consumer's decisions or work. Replace unsupported universal claims with conditional guidance. Keep relevant project policies, not a previous project's incidental filenames, paths, or review system.

**Done when:** requirements are covered, changes stay in scope, placeholders are intentional only in fillable templates, and references are checked or explicitly unresolved.

### 6. Exercise when required; deliver the current result

When testing is requested or a consequential behavioral ambiguity needs checking, use current instructions with representative inputs. Include a relevant missing-input or conflicting-control case. Construct it from real prerequisites, not additional requirements invented for the test. Before attributing a result to the instructions under evaluation, identify which decisions, safeguards or construction steps the task and shared resources already supply; strongly scaffolded compliance does not establish independent instruction benefit or spontaneous discovery.

For independent-consumer verification, use `delegate_task` with only the declared resources, task inputs, role, permitted scope, and completion criteria. Pass exact paths for file-based inputs and outputs; otherwise supply the actual input and conversation-delivery requirement. Do not add design history that conceals a missing prerequisite. Inspect the actual returned document or authorized artifact, not the worker's success claim. When input isolation matters, compare the actual assignment, declared inputs, read records and available completed tool trace. Distinguish visible permitted reads from independently established full consumption: abbreviated arguments/results and unavailable ambient context limit what the trace proves. Report those limits rather than certify perfect isolation from separate worker identifiers or self-reports.

Supply actual review returns and revision limits only where the review contract requires them. Reconcile recommendations into one authoritative instruction. After a correction, rerun affected checks; earlier results apply to the revision actually consumed. Preserve read-only review boundaries and distinguish authored fixtures from real-source acquisition or end-to-end execution.

Return inline drafts or read-only review findings in conversation when that is the requested delivery. Use `write_file` or `patch` only for authorized file outputs. Report authored, inspected, behaviorally exercised, and owner-accepted separately; a draft or test pass is not human approval. Honor an explicit owner stopping decision without manufacturing more work.

**Done when:** the current document is reachable, requested checks were performed or their limits disclosed, and unresolved owner decisions remain visible.

## Example

**Illustrative request:** Write instructions for an agent to summarize a reading group's venue decision from supplied notes. Include follow-up owners and deadlines; record missing details explicitly. Return the instructions in conversation. Notes: “Use the library room. Alex will check availability.”

**Complete output:**

```markdown
# Summarize the venue decision

## Purpose and scope
Prepare a venue summary from the supplied reading-group notes. Record what was
said; choosing a date, booking a room, and contacting participants are outside scope.

## Inputs
- Reading-group notes, supplied in the assignment. If absent, ask for them before
  summarizing. The illustrative notes are: “Use the library room. Alex will check
  availability.” Future runs use the notes supplied for that run.
- Owners and deadlines only when stated in those notes. Use “Not recorded” for
  either missing value; do not infer a deadline from the event date.

## Procedure
1. Extract the recorded venue decision and each follow-up action, retaining any
   stated owner and deadline.
   Done when: every venue decision and follow-up in the notes is accounted for.
2. Populate the output structure below. If no venue decision appears, write
   “None recorded”; if there are no actions, write “None recorded” instead of a table.
   Done when: all extracted entries are represented and missing values are explicit.

## Output
Return a Venue Summary in conversation:

# Venue Summary
## Recorded venue decision
[State the recorded venue decision, or “None recorded”.]
## Follow-up actions
| Action | Owner | Deadline |
|---|---|---|
| [Recorded action] | [Stated owner or “Not recorded”] | [Stated deadline or “Not recorded”] |

For the supplied illustrative notes, the filled result is:

# Venue Summary
## Recorded venue decision
- Use the library room.
## Follow-up actions
| Action | Owner | Deadline |
|---|---|---|
| Check library-room availability | Alex | Not recorded |

## Checks and stopping
Compare each entry with the notes. Preserve the decision and owner, account for
all follow-ups, and invent no deadline or booking confirmation. Stop after returning
the summary; this assignment authorizes no external action.
```

## Pitfalls

- Improving wording while leaving an input, output, or completion gate undefined.
- Applying one role's restriction to every phase, or retaining a superseded decision.
- Making a portable guide depend on neighboring design notes or sources not declared as consumer inputs.
- Presenting a method, metaphor, or intended audience effect as proven universal behavior.
- Testing with extra author context or attributing a pre-edit test to revised instructions.
- Diagnosing a malformed authored placeholder from a tool-masked first token. When presentation redaction obscures a field, check its actual delimiter structure programmatically without exposing secret values before treating the display as a document defect; tool masking can make an intact bracketed placeholder appear broken.
- Requiring byte identity for an ordinary prose repair. Use semantic inspection and focused diffs; literal comparisons serve explicit quotation, copying, or immutability requirements.

## Verification

- Consumer, task, input contract, output, and scope are explicit.
- Every material requirement maps to an action, output element, or constraint.
- Missing and conflicting inputs have defined behavior; conditional sections have triggers.
- Each procedural step has an observable completion criterion without adding prerequisites.
- Templates define content and field meanings; complete examples obey the same factual boundaries.
- Pointers name targets, read conditions, and path bases; checked targets and headings resolve.
- Revision inspection establishes preserved meaning and scope, not only formatting or integrity.
- Reported behavioral results come from actual execution of the identified revision; limitations and human decisions remain visible.
