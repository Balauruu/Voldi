---
name: show-me
description: "Use when a topic needs a concise visual explanation."
version: 1.0.0
author: Dex Horthy (origin), Balauru, Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [visual-explanation, diagrams, code-shapes, html]
    related_skills: []
  credits:
    author: Dex Horthy
    organisation: HumanLayer
    url: https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md
---

# Show Me

Skip the preamble; keep prose brief. Pick the smallest useful visual that answers
one current question. Explain existing or explicitly proposed structure; this
skill does not authorize implementation, publication, or a renderer subsystem.

## When to Use

- A user wants to see logic, runtime flow, ownership, structure, or a change.
- A current discussion would be clearer as a diagram or focused UI/concept view.
- Don't use for an unrelated overview, an ornamental diagram, or an entire app.

## Prerequisites

Text visuals need no renderer or install. Use `read_file`, `search_files`, or
retrieval tools only for sources the chosen explanation needs. Use
`browser_exec` for browser verification and `vision_analyze` for captured images
when rendered readability is required; read the bundled
[Mermaid rendering reference](references/mermaid-rendering.md) before that branch.
Do not install frameworks, browser engines, or a rendering pipeline by default.

## Inputs

- **Topic/question (required):** use the explicit request or unambiguous current
  discussion. Ask one focused question if multiple topics remain plausible.
- **Needed sources:** supplied text, code, documents, screenshots, data, or URLs
  supporting the selected claims. Read their relevant complete units and context;
  retrieve an identified missing source if permitted. Ask if a load-bearing source
  cannot be obtained; otherwise omit unsupported detail or mark it unknown.
- **Structure status:** distinguish supplied/observed facts from proposed design.
  A proposal needs a requested design goal; do not silently invent actual behavior.
- **Optional controls:** audience, before/after baseline, output schema, destination,
  rendering requirement, frozen source, product style, viewport, and network limits.
  Preserve these controls; absence is not permission to edit a source or publish.

Never invent facts, file paths, labels, data, timings, or causal relationships.
Use identifiers and paths exactly as supported. Explicitly proposed identifiers
are allowed only as design choices, not discovered files. Label examples as
illustrative; missing invalidation, order, or ownership stays unknown.

## Procedure

1. **Bind the explanation.** Identify the question, needed sources, and governing
   controls from the inputs. Read source material before drawing its structure.
   **Done when:** one question is settled and every load-bearing source is read
   or its absence triggers a question/explicit limit rather than invented detail.
2. **Extract only the answer-bearing shape.** Retain relevant calls, files, props,
   states, owners, boundaries, and relationships. Separate known from proposed;
   use distinct labels or panels when both appear. Preserve unknowns.
   **Done when:** every retained element is supported or explicitly proposed.
3. **Choose the smallest representation.** Use the table below; combine views
   only when each answers a different necessary part of the question.
   **Done when:** removing another view would lose needed meaning, not decoration.

   | Question/shape | Representation and boundary |
   |---|---|
   | Logic or algorithm | Pseudocode; preserve branches, early returns, and order. |
   | Runtime control flow | Call tree; indentation means caller/callee, not ownership or an unproven execution order. Annotate supported order if it matters. |
   | UI/state/module ownership | Component, state, or module tree; label containment, state owner, and cross-module boundary where needed. Do not imply a hook/component tree is a call stack. |
   | File responsibility/broad refactor | Shallow file tree with short responsibility comments; omit unrelated directories. |
   | Change to an existing shape | Focused `diff` matching that shape: component, file tree, call tree/stack, state, or control flow. Name before/after status; do not claim it is an applied patch. |
   | Most of a block is new, omission hides ownership/order, or the user needs a copyable target shape | Full copyable target block; no ellipses hiding essential context. Distinguish runnable code from conceptual sketches. |
   | Interactions/control/data flow | Mermaid; name arrow meanings (request, response, data, dependency, or explanatory progression) and supported directions. |
   | Visual UI/layout/state comparison, or concept too dense for Mermaid | One focused HTML file: diagram, infographic, or short slide deck. Not a generated app. |

4. **Construct the Visual Answer.** Put the visual next to its short explanation.
   Create Visual Answer with this complete structure; substitute content and
   remove conditional lines when their condition does not apply. An owner-supplied
   schema or artifact-only return takes precedence; do not append a second report.

   ````markdown
   **[Known, Proposed, or Illustrative] — [Specific question answered]**

   ```[text, diff, code language, or mermaid]
   [Complete smallest useful visual with supported identifiers and relationships.]
   ```

   [One short explanation of the point; include indentation/arrow meaning if ambiguous.]
   Source: [Supplied input or exact source locator(s); only if grounding is not already clear.]
   Unknown: [Only material missing facts; omit if none.]
   Render: [Only if requested/attempted: inspected result, failure, or not checked; never infer success.]
   [For HTML/file delivery instead of a fence: the actual artifact attachment or supported preview.]
   ````

   For HTML, replace the visual fence with the delivered artifact; put scope,
   legend, and material unknowns inside it if the return must be artifact-only.
   **Done when:** the complete answer exists, no placeholders remain, and it does
   not require a second explanation to discover what its structure means.
5. **Check semantics independently.** Compare every node, branch, relationship,
   state owner, path, and change against the sources. Confirm omissions do not hide
   ownership or order. Explicitly distinguish temporal progression from causation,
   transmission, dependency, and required sequence. Correct unsupported claims.
   **Done when:** the visual answers the bound question without changing its facts.
6. **Check rendering when applicable.** If readability is requested or HTML is
   delivered, inspect the actual artifact at relevant desktop/mobile sizes. For
   Mermaid use the reference: actual source, DOM/error checks, capture, then
   `vision_analyze`. Parsing is not visual inspection; visual inspection is not
   semantic proof. If unavailable, state the unverified condition or blocker.
   **Done when:** labels, connectors, contrast, clipping, and consequential overlap
   are inspected, or the precise remaining limitation is visible.
7. **Deliver without a preamble.** Return the Visual Answer or requested artifact.
   In Hermes Desktop, attach a verified file with `MEDIA:/absolute/path/to/file`;
   optionally open it with `desktop_preview(action="open", url=<actual path>)`
   when that tool is available. Verify the exact preview target after opening.
   Otherwise use the surface's supported file mechanism and an exact local path.
   **Done when:** the reader can reach the actual result and no unverified render
   or proposed implementation is described as completed execution.

## HTML Surface Rules

- Match supplied product colors, type, spacing, and components; use real labels
  and data. Without product styling, choose a restrained standalone style.
- A Desktop inline HTML widget uses inherited theme variables; do not override
  its background, font, or margins, or reset the host. Scope internal layout CSS.
- A standalone HTML page may own background, typography, and page margins. Do not
  apply the standalone reset/style rules to an inline widget.
- Support desktop/mobile readability and accessible labels/contrast. Keep one
  focused artifact and its assets reachable; no framework installation by default.

## Complete Illustrative Example

This is a teaching fixture, not observed repository behavior or an executed test.

**Input:** “Show the save behavior as pseudocode, not executable code. If content
is unchanged, return the cached result. Otherwise write new content and return a
fresh result. Cache invalidation was not specified.”

**Complete output:**

````markdown
**Known — supplied save behavior**

```text
on(save)
  if content is unchanged
    return cached result
  write new content
  return fresh result
```

The unchanged branch returns before writing; indentation shows control flow.
Source: supplied behavior.
Unknown: cache invalidation.
````

## Pitfalls

A tree is not a timing trace; a diff sketch is not an applied patch. A progression
arrow does not prove influence. A pretty render can still misstate the source.
Temporary viewers/screenshots are verification aids, not required product assets;
never alter a frozen source to make an inspection image prettier.

## Verification

- The one bound question is answered with the smallest useful view, no preamble.
- Every element is sourced or explicitly proposed; material unknowns remain visible.
- The representation, ownership/order, legend, and known/proposed labels agree.
- The applicable output contract is complete and the artifact is reachable.
- Semantic checks and rendering status are separate; no invented execution results.
- Source freezes, surface theming, delivery scope, and no-install default are kept.

Derived from Dex Horthy's HumanLayer skill; adapted by Balauru and Hermes Agent.
The upstream MIT copyright notice and grant are retained in [LICENSE](LICENSE).
