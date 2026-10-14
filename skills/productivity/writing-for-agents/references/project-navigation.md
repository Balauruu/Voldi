# Project navigation and artifact placement

Use this recipe when creating a project index, assessing index/context overlap, or changing where instruction-led work produces its artifacts.

## Inputs and output

Required: the project root, current folder/resource contents, requested operation, and any supplied template or placement policy. Resolve relative input paths from the project root; links in the resulting index are relative to that index. Ask for an inaccessible required project root or an unresolved placement decision; report an unrelated broken link without restoring its target.

A new index is `<project-root>/INDEX.md` unless the owner supplies another destination. Existing-document reviews return located findings without editing. Placement changes and moves require their own explicit scope; index creation alone authorizes neither.

## Create a root index

1. Read the project's instructions, terminology, current design and controlling owner decisions. Load the owner's supplied index template with `read_file` before drafting; preserve its useful section structure rather than inventing a new documentation scheme.
2. Inspect immediate project folders and the proposed canonical files. Read enough of each resource to verify its role, not merely its existence. Keep generated dependencies out of domain routes and identify archives, withdrawn prototypes, diagrams and demo inputs by their actual status.
3. Create Project Index with the following sections, adapting an owner-supplied template when it differs. Use paths relative to the index. List entry points and concerns, not every generated file or session. Omit empty tables with “None identified”; explicitly report unresolved authority instead of inventing a canonical file.

   ```markdown
   # Project Index

   ## Folder Map
   | Folder | Role |
   |---|---|
   | [Existing relative folder path] | [Verified purpose, not an inferred status] |

   ## Canonical Files
   | Resource | Authority |
   |---|---|
   | [Link to an existing file] | [Concern for which this file is authoritative] |

   ## Where To Go
   | Task | Start here | Additional reads |
   |---|---|---|
   | [Concrete task trigger] | [Exact starting file link] | [Conditional pointer with read reason, or “None”] |

   ## Navigation Rules
   [Explain path bases and applicable precedence or boundary rules.]

   ## Maintenance
   [State what layout or authority changes require rechecking this map.
   Include a verification date only if requested and actually obtained.]
   ```

   Done when: every listed role is supported by inspected contents and every route identifies an actual starting resource.
4. Give each route a concrete starting file and explicit conditions for additional reads. Distinguish required reads from optional provenance inspection; an index's read-next column must not weaken prerequisites in the linked resource.
5. Keep routes stable: point to primitives, instruction resources and artifact categories, not named sessions/trials, tickets or pending implementation work. Resolve concrete session, kit and material selections from the task. Add named-artifact inspection routes only when the owner explicitly asks for an inspection map. Keep a project's research/demo provenance in the actual material, so a routing file neither certifies facts nor turns a fixture into a required input directory.
6. Verify local targets and referenced headings and remove template placeholders. If a verification date is requested, obtain it with `terminal`; otherwise omit it. A small Python check can extract Markdown targets with `re.findall(r'\]\(([^)]+)\)', text)`, deduplicate them, resolve them from the index directory and assert existence. Check fragment anchors and section names as well when the map uses them. Split a target with `target, _, fragment = href.partition('#')`; resolve a nonempty target from the containing file's directory, but resolve an empty target to the containing file itself. A `#heading` link names that document, not its directory; validate its fragment against the document's actual heading anchors. If verification exposes an unrelated missing target or owner deletion, distinguish it from changed-link failures and report it; do not restore content or broaden edits merely to make the checker green.
7. Deliver the index at its requested entry point. Update pointers in ordinary task documents only when those files are included in the authorized scope. An index-only request leaves other documents unchanged; creating an index does not establish automatic discovery.

## Complete illustrative index example

**Input:** An illustrative project contains `notes/decision.md`, stating the recorded venue choice, and `procedures/venue-summary.md`, defining how to summarize venue notes. The owner names the former authoritative for the venue decision and the latter authoritative for summary preparation. Create an index only; no additional read is needed for either route. These are example premises, not claims about files on this machine.

**Output:**

```markdown
# Project Index

## Folder Map
| Folder | Role |
|---|---|
| `notes/` | Recorded project decisions |
| `procedures/` | Instructions for preparing project documents |

## Canonical Files
| Resource | Authority |
|---|---|
| [Venue decision](notes/decision.md) | The recorded venue choice |
| [Venue-summary procedure](procedures/venue-summary.md) | How to prepare a venue summary |

## Where To Go
| Task | Start here | Additional reads |
|---|---|---|
| Find the recorded venue choice | [Venue decision](notes/decision.md) | None |
| Prepare a venue summary | [Venue-summary procedure](procedures/venue-summary.md) | None |

## Navigation Rules
Paths are relative to this index. The decision file records the choice; the
procedure defines how to summarize it. This map introduces no new decisions.

## Maintenance
Recheck this map when either resource moves or its authoritative role changes.
```

## Decide whether index and context files overlap

Compare responsibilities, not shared keywords or arbitrary similarity percentages:

| Responsibility | Navigation index | Stage context contract |
| --- | --- | --- |
| Folder concern, authoritative paths and task routing | Primary job | Only the local orientation needed for the stage |
| Required inputs, artifact ownership, preservation and promised outputs | Points to their authoritative resource | Local Input / Do / Output interface |
| Detailed domain procedure | Points to the procedure | Points to the procedure |
| Collection-specific findings or execution state | Not its job | Not a stable contract's job |

Keep a root map and lean stage contracts. Add a lower-level index only when it routes genuinely distinct concerns not already mapped by the local contract. Trim procedural duplication between contracts, coordinator guidance and domain procedures before replacing a useful contract with a navigation-only template.

## Change placement instructions for future work

1. Resolve the actual index path from supplied project inputs, including an owner-selected filename, and read it if it exists. Read the authoritative placement-policy document and latest owner direction. If no index exists, update only the authorized policy document; create an index only when requested. Ask when policy authority cannot be determined. For repository work, inspect current changes with `terminal(command="git status --short")`. Identify permissive destination wording as well as missing guidance; an added prohibition cannot reliably override an unchanged allowance.
2. Record the concrete placement policy in its existing authoritative document. Keep task-conditional conventions in that home; do not add a redundant reminder to an always-loaded orientation file such as `AGENTS.md` when its existing index route already makes the convention discoverable. A requirement to keep orientation current does not require changing it when its responsibilities and routes are unchanged. When an existing index is included in the authorized scope, update affected folder roles, task routes and the policy pointer there together. If updating an ordinary task handoff is authorized, point to the policy before its artifact-generation step. Otherwise leave these documents unchanged and identify any needed follow-up repair. Keep examples generic rather than selecting a past exercise as the default.
3. Create a newly linked directory only when needed to make the declared layout real. An empty `.gitkeep` can retain a directory without inventing another README or report. Treat a proposed `/tests` name as project-relative when the owner explicitly requires in-project work.
4. Make the current rule's precedence over older destination allowances explicit. A future-placement correction does not authorize moving or deleting existing external artifacts; leave them untouched unless relocation is requested.
5. Check new directory links and heading anchors. For repository work use `terminal(command="git diff --check")`, and report instructions changed separately from artifacts moved. Preserve unrelated working-tree changes and disclose any pre-existing broken links.

## Relocate generated artifacts coherently

1. Confirm that relocating existing artifacts is explicitly requested, then confirm the intended repository-relative location and the latest owner override. Identify affected artifact producers, stage contracts, coordinator handoffs, consumers and design notes; folder relocation alone does not change the instructions that generate the next output.
2. Identify the artifacts to move and any expressly protected content. Check the destination before moving so a layout change cannot overwrite another artifact. For an unchanged-content move, use a targeted before/after comparison if needed; reserve hashes for a concrete integrity need rather than baseline unrelated prose or source material.
3. Move only the authorized existing artifacts without regenerating their content or overwriting destination files. Remove obsolete directories only when empty; preserve unrelated work. Done when: the selected artifacts are at the intended destination and unrelated artifacts remain outside the move.
4. Update existing, affected pointers and default locations only within the authorized scope. Stage contracts, session copies and design notes are conditional resources, not prerequisites: skip absent or irrelevant types and report a necessary out-of-scope pointer repair rather than performing it. Preserve historical decisions as historical; mark an affected old instruction superseded where appropriate. Done when: authorized active pointers use the new location and unresolved external repairs are identified.
5. Preserve the consumer-input boundary. Moving a portable guide does not make neighboring contracts, analyses or sources new runtime dependencies. Verify sibling links after moving related files together. Done when: consumers can reach the moved resources with the same declared inputs, or a genuine interface change is reported for a decision.
6. Verify retained meaning/content, active pointers and relative links. Use literal comparison for an explicitly unchanged-content move, not a repository-wide baseline. If a move affects an existing helper's resource bindings, exercise that declared helper with its actual inputs in an authorized disposable layout and inspect the promised behavior; ask for unavailable required helper inputs. Do not invent a factory layout, helper or creative rerun for an ordinary document move. Done when: the changed entry path is checked at the relevant level and any unexercised boundary is disclosed.
7. Report what moved, which instructions changed, what was verified and what remained untouched. Do not manufacture new reports, registries or workflow machinery for a layout correction.
