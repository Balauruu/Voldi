# Render and inspect the actual Mermaid source

Use only when rendered readability must be verified. This reference describes a
recipe, not an assertion that it has run on the current source. It does not
introduce a product pipeline, installation requirement, or required companion file.

## Inputs and authority

- Exact source document and the selected fenced Mermaid block (zero-based index
  only as an extraction selector; identify its subject in the report).
- Surrounding explanation, especially arrow meanings and known/proposed status.
- Editing/freeze rule, permitted temporary-output directory, allowed browser/network
  route, theme/surface, and intended viewport(s). Resolve these before rendering.
- If the document is frozen, retain its bytes/digest before inspection and compare
  afterward. A rendering aid cannot authorize source edits, new product assets,
  publication, or installation. Read the complete document with `read_file` first.

## Ordered procedure

1. **Extract, don't reconstruct.** Extract the exact required `mermaid` fence from
   the read document. Retain its surrounding explanation. If several blocks exist,
   select explicitly; render every block required by the task rather than silently
   choosing the first. Do not substitute a similar graph or invented labels.
   **Done when:** the selected source and its contextual claims are identified.
2. **Bind the viewer.** Use `browser_exec` with a named isolated session, reusing
   that name for all related calls. Initialize with `new_tab` on first navigation.
   Use an already permitted renderer when available; otherwise the recipe below
   fetches Mermaid from a CDN only if network policy permits. Do not upload private
   source to a rendering service. If browser/CDN access fails, use an existing
   permitted route or report the blocker; do not install a renderer by default.
   **Done when:** the allowed renderer and output location are settled.
3. **Render exact source and expose failure.** Serialize with `json.dumps`, never
   hand-escape source into JavaScript. Initialize and render within the same call,
   avoiding stale globals across navigation. Catch import/parse/render errors.
   This illustrative tool-call body must be filled with actual authorized inputs;
   it has not been exercised as part of authoring this reference.

   ```python
   # Rendering the selected source Mermaid block
   import json, re, hashlib
   from pathlib import Path
   source_path = Path("REPLACE_WITH_ACTUAL_SOURCE_DOCUMENT")
   selected_index = 0  # Set from the explicitly selected block, not a default guess.
   raw = source_path.read_bytes()
   source_digest = hashlib.sha256(raw).hexdigest()
   text = raw.decode("utf-8")
   blocks = list(re.finditer(
       r"(?m)^ {0,3}(?P<fence>(?P<mark>`|~)(?P=mark){2,})[ \t]*mermaid"
       r"(?:[ \t]+[^\n]*)?\r?\n(?P<body>.*?)"
       r"^ {0,3}(?P=fence)(?P=mark)*[ \t]*\r?$",
       text, re.S,
   ))
   if not blocks or not 0 <= selected_index < len(blocks):
       raise ValueError("Selected Mermaid fence not found; inspect source format")
   source = blocks[selected_index].group("body")
   new_tab("about:blank")
   result = js("""(async () => {
     try {
       const mod = await import('https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs');
       const mermaid = mod.default;
       mermaid.initialize({startOnLoad:false, securityLevel:'strict',
         theme:'default', flowchart:{htmlLabels:true, useMaxWidth:false}});
       document.body.innerHTML = '<main id="diagram"></main>';
       document.body.style.background = 'white';
       document.body.style.color = '#111';
       const rendered = await mermaid.render('inspectionMap', SOURCE_JSON);
       document.querySelector('#diagram').innerHTML = rendered.svg;
       const svg = document.querySelector('#diagram svg');
       if (!svg) throw new Error('Renderer returned no SVG');
       const r = svg.getBoundingClientRect();
       return {ok:true, renderer:'mermaid@11 CDN',
         viewBox:svg.getAttribute('viewBox'),
         bounds:{x:r.x,y:r.y,width:r.width,height:r.height},
         labels:[...svg.querySelectorAll('text, .nodeLabel, .edgeLabel')]
           .map(e => e.textContent.trim()).filter(Boolean)};
     } catch (error) { return {ok:false, error:String(error)}; }
   })()""".replace("SOURCE_JSON", json.dumps(source)))
   print(json.dumps({"source":str(source_path), "sha256":source_digest,
                     "block_index":selected_index, "render":result}))
   ```

   Run through `browser_exec(code=<filled body>, session=<chosen name>)`.
   Ordinary unindented/top-level fences with up to three leading spaces are covered, including longer matching closers. Container/nested or other unsupported fence formats are a
   reason to inspect extraction, not to redraw the graph. Do not strip meaningful
   source text for convenience. `mermaid@11` is a major-version URL, not an exact
   build pin; record the actual route and any resolved version available for repeat
   comparisons. No SVG, `ok:false`, a saved source, or an error page is not success.
   **Done when:** actual SVG exists or the exact failure is retained.
4. **Inspect DOM before pixels.** Use `page_info()` and `js()` to read actual labels,
   SVG `viewBox`, element bounds, and visible error text. Match nodes/relationships
   to the selected source and explanation. Nested label containers can repeat text;
   deduplicate content for comparison, not as a claimed node/edge count. Sequence
   and other diagram types have different DOM shapes; do not assume flowchart-only
   selectors identify all entities. Parsing and text extraction are not readability.
   **Done when:** source content is accounted for and failures are not hidden.
5. **Capture the actual rendered view.** Include the arrow legend or relevant
   explanatory note beside the graph. Match viewer colors to Mermaid's theme; a
   dark inherited background can hide dark connectors while light nodes remain
   visible. The recipe owns a temporary standalone blank page, not a Desktop inline
   widget; inline widgets inherit theme variables and must not reset background,
   font, or margins. Size the viewport around SVG plus its note using bounds, not
   an oversized empty canvas. Use `cdp('Emulation.setDeviceMetricsOverride', ... )`
   for required sizes. Capture via `capture_screenshot()` or raw CDP.

   If the helper's short response timeout fails after rendering, retry with:

   ```python
   # Capturing the actual rendered Mermaid viewer
   import base64
   from pathlib import Path
   image_path = Path("REPLACE_WITH_AUTHORIZED_CAPTURE_PATH.png")
   capture = cdp('Page.captureScreenshot', format='png',
                 captureBeyondViewport=False, fromSurface=True,
                 _response_timeout=60)
   image_path.write_bytes(base64.b64decode(capture['data']))
   print(str(image_path))
   ```

   Save/copy captures only into the declared permitted temporary-output location;
   a helper's shared screenshot filename is not a durable handoff. Honor a required
   project-local temporary location instead of substituting host scratch. Do not
   overwrite another task's screenshot. Browser Python variables do not persist
   across calls: retain values in authorized files/output or re-read inputs.
   **Done when:** a real image of the correct target and viewport is available.
6. **Inspect pixels and semantics separately.** Call `vision_analyze` on the capture
   alongside the source explanation. Check required node/relationship labels,
   arrow directions, solid/dotted connector visibility, contrast, clipping, and
   consequential overlap. Small crossings are acceptable when endpoints and labels
   remain unambiguous. If tiny labels cannot be assessed, inspect a closer crop.
   **Done when:** readability is actually assessed, not inferred from parsed SVG.
7. **Repair only within authority.** Fix viewer-only contrast, canvas, or sizing
   defects, recapture, and reinspect. For sequence labels obscured by a lifeline,
   try a viewer-only `.messageText` halo: `paint-order: stroke`, a stroke matching
   the viewer background, `stroke-width: 4px`, and `stroke-linejoin: round`.
   Apply it only after inspecting the actual overlap; recapture and confirm the
   halo preserves readable labels, connector endpoints and directions, because
   masking improves text contrast but can also hide nearby diagram strokes.
   Do not require dotted lifelines or another theme convention unless the source
   or owner specifies it; renderer defaults are not semantic obligations.
   A semantic error or inherent unreadability is
   a source defect: follow editing/approval rules or report it. Never silently edit
   frozen source or present a redrawn substitute as its inspected render. Compare
   frozen bytes/digest afterward using the original retained value; disclose drift.
   **Done when:** source preservation and any remaining defects are accounted for.
8. **Deliver the requested source/result.** Keep temporary viewers outside product
   assets when embedded Mermaid is the deliverable. An inspected image can be an
   optional attachment, not a new mandatory companion. Verify actual file/preview
   targets, then use `MEDIA:/absolute/path/to/file` in Desktop or the supported
   surface mechanism. Report separately: semantic result, render/readability result,
   source preservation, and remaining limitations. A successful render establishes
   neither causal truth, writing quality, implementation success, nor acceptance.
   **Done when:** the actual requested result is reachable without scope expansion.
