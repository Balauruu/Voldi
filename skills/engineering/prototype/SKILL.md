---
name: prototype
description: Use when prototyping code, UI or agent instructions. Build bounded artifacts for design feedback without automatic production integration.
---

# Prototype

A prototype is **a bounded, non-production artifact that answers a question**. The question decides the shape; a request for instruction construction does not require code or a simulator.

## Pick a branch

Identify which question is being answered, using the user's prompt, the surrounding code, or by asking if the user is around:

- **"Does this logic / state model feel right?"** → [LOGIC.md](LOGIC.md). Build a single shareable HTML file (free-play buttons plus tabbed guided walkthroughs) that pushes the state machine through cases that are hard to reason about on paper, and that a non-developer can drive.
- **"What should this look like?"** → [UI.md](UI.md). Generate several radically different UI variations on a single route, switchable via a URL search param and a floating bottom bar.
- **"What should these agent instructions or skill contracts contain?"** → Load `writing-for-agents` and produce clearly marked Markdown artifacts, not a UI/state-machine simulator. Keep them outside installed skill paths and avoid generated `SKILL.md` filenames when production skills are not authorized. A separately supplied existing core remains an input, not a new prototype output. Branch/issue publication and installation require task authorization.

  1. Bind the existing owner decisions, exact permitted inputs/outputs, role access and question still requiring feedback. Use an approved alternative as a comparison control without reopening the settled default.
  2. Build the requested depth. A sketch-only request needs a sketch; a requested extraction → synthesis → portable guidance → application chain needs filled actual artifacts at every seam. Cheap means avoiding unrelated implementation, not omitting required construction.
  3. Exercise only the authorized scope. For representation comparisons, hold substantive obligations, factual premises, core and role boundaries constant rather than weaken the control. Inspect complete actual applications, fitting alternatives/declines and affirmative repairs. Label illustrative teaching, executed bounded application and full-workflow evidence separately; account for task/core scaffolding before claiming guide benefit.
  4. Load `deliver-work`, deliver the actual artifacts and complete comparison through the authorized surface, then seek the required human reaction. Keep optional editorial advice separate from mandatory defects; preserve executed returns rather than silently polish them into new evidence. Record actual acceptance, required revisions and accepted limits before satisfying an owner-feedback completion gate.

The branches produce very different artifacts, so getting this wrong wastes the whole prototype. If the question is genuinely ambiguous and the user isn't reachable, default to whichever branch better matches the surrounding code (a backend module → logic; a page or component → UI) and state the assumption at the top of the prototype.

## Rules that apply to both

1. **Throwaway from day one, and clearly marked as such.** Locate the prototype code close to where it will actually be used (next to the module or page it's prototyping for) so context is obvious, but name it so a casual reader can see it's a prototype, not production. For throwaway UI routes, obey whatever routing convention the project already uses; don't invent a new top-level structure.
2. **Trivial to run.** A UI prototype starts from one command in the project's task runner: `pnpm <name>`, `python <path>`, `bun <path>`, etc. A logic demo is a single HTML file the user double-clicks. Either way, no thinking required to start it.
3. **No persistence by default.** State lives in memory. Persistence is the thing the prototype is _checking_, not something it should depend on. If the question explicitly involves a database, hit a scratch DB or a local file with a clear "PROTOTYPE, wipe me" name.
4. **Skip unrelated polish, not verification.** Avoid production-hardening, speculative abstractions and tests unrelated to the question. Exercise the behavior and checks needed to answer that question, including the user's explicit acceptance criteria; a runnable shell or plausible sketch alone cannot establish the result.
5. **Surface the state.** After every action (logic) or on every variant switch (UI), print or render the full relevant state so the user can see what changed.
6. **Capture the answer within authorized scope.** Retain the prototype and its findings in the owner-approved artifact/canonical home, with a reachable route and explicit limits. Commit to a throwaway branch only when branch publication is authorized; use the permitted local or native-attachment route otherwise. Fold a validated decision into production only under separate implementation authority, because prototype evidence or owner design acceptance does not grant production-edit permission.
