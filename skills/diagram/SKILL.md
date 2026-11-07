---
name: diagram
description: Make a monochrome dimensional drawing of a UI component or physical product. Two modes — diagram (default; a calm line figure with a few real dimensions and short labels or notes — front views, before/after, sequences, token scales, oblique or flat exploded stacks — for articles and docs) and spec sheet (Apple-style title block plus one dense sheet with variable-coded dimensions and tables). Use when asked for a dimensional drawing, diagram, figure, anatomy, exploded view, spec sheet or redline of a component.
---

# Diagram

## Pick a mode

- **Diagram mode (default).** One idea per figure, a few dimensions with real values, nothing else. Use it for "diagram", "figure", "drawing", "exploded view", article or doc illustrations, and whenever the user doesn't ask for a spec. Rules are in **Diagram mode** below; the worked example is `examples/card-diagram.html`.
- **Spec mode.** The full sheet: title block, notes, many views, variable codes and tables. Use it only when the user asks for a spec sheet, redline, full anatomy or a handoff document. Everything from **Spec mode** on applies.

Both modes share the house rules on monochrome, architecture over content, unbranded and never inventing values.

## Diagram mode

Simple, calm line figures, the kind that sit inside an article at full column width. Fewer marks always beats more.

**Content**
- One idea per figure. Make a second figure for a second idea instead of crowding the first. The default is one figure: the front view. Add another view only when it shows something the front can't.
- Figure types, pick the one that fits the idea:
  - **Front view** of a component or object with a few dimensions.
  - **Before / after** or a **state change**: two states joined by a labeled arrow (`UPLOAD →`, a vertical `LOUD ↓`).
  - **Sequence**: the same layout at steps (`n = 1`, `n = 9`, `n = 25`), each with a mono caption.
  - **Order or path**: a muted path line with rounded turns and an arrowhead traced through the cells, axes labeled `+x` / `+y`.
  - **Token scale**: a row of swatches with mono step labels under them (`50 … 950`), spacing tokens as bars with the mono name left and the muted value right (`space.4  16`), and when the progression matters, a line with dots over faint dotted gridlines above the swatches.
  - **Exploded view**, only when the user asks or the layering is the point (real parts, surfaces whose order matters). Never by habit. Two styles, chosen by the user or by the subject:
    - **Oblique stack** (default for UI layers and abstract layers like token tiers): each layer drawn flat in its own units, then projected with one shared oblique matrix; even vertical gaps; faint dash-dot projection lines through the outer corners from the top layer to the bottom; caps labels in a column on the left, each with a horizontal leader ending in a dot on that layer's leftmost corner. Labels read top to bottom in stack order.
    - **Flat side elevation** (default for physical products): parts as flush horizontal profiles stacked with even gaps, one thin axis through the stack, leaders to circled numbers in a right-hand column with caps part names, numbered from the bottom up.
  - **Side view** of a physical object, beside the front at the same scale and aligned on a shared baseline.
- No detail circles, no radius quarter-arcs, no isometric of a single object for its own sake.
- 2–5 dimensions per figure, only the ones the reader needs. Labels are the real value (`64`, `128.00`, `W = 420–680`) or a short letter/formula when the figure is about the rule (`W`, `G`, `P`, `5P − G`). No code tables, no variable names, no derived-value formulas on the drawing.
- **Labeling**, two systems, never both on one feature:
  - **Direct labels** name a part: caps sans name, leader with a short shoulder, small dot on the feature. Use them when the name says it all (`GRAY FILL`, `DROP SHADOW`). Up to ~6 per figure, placed on both sides so leaders don't cross.
  - **Boxed note numbers + notes list** when the point needs a sentence: at most 4–5 plain sentences under a thin rule. Boxes go in clear space (inside padding, inside a gap, beside a dimension label), never on an outline, a rounded corner or another label.
- No title block, no sheet border, no footer, no drawing number. A single caps caption under each view (`FRONT · W = 600`, `T0 · BEFORE`) replaces all of it; skip the caption when labels already say what the figure is.
- Size the subject to read at article width: aim for 600–800 viewBox px wide. Draw small UI at 2:1 (or 3:1) by multiplying every coordinate, never with a `transform` on flat views (line weights must stay true), and put the scale in the caption (`FRONT · 2:1`). Dimension labels always carry source values. Oblique layers are the one transform; they use `vector-effect: non-scaling-stroke`.

**Source values.** From Figma, read the node with `get_metadata` (boxes) and `get_design_context` (padding, gap, radius, fills); the component description often states the grid and sizing intent. From code, measure the rendered box tree. Text widths are content, so draw text as full-width slots.

**Line language** (three weights, nothing between)
- Object outline 2 px, surface fill. Inner parts and features 1.25 px. Dimension lines and leaders 1 px in ink; extension lines, axes, projection lines and gridlines 1 px in the faint gray.
- Dimensions use small filled arrowheads with faint extension lines (spans too short for two heads get them outside, pointing in), and sit 30–60 px off the object. End ticks instead of arrowheads for physical products, or when the user prefers them.
- **One focus element** per figure gets the solid ink fill (the thing the figure is about: the newest item, the primary action). Everything else is outlined.
- Fills, each used sparingly: dashed faint outline for placeholders and empty slots; mid gray for a part that reads as a different material (text, a colored inset, a well); 45° hatch for shadows, batteries, keep-outs.
- Axes and centerlines: faint dash-dot, labeled in mono when they carry meaning (`y = 0`, `x = −1`, `+x`). Paths: 1.5 px muted, rounded corners, one arrowhead at the end.
- Labels inside tiles (index numbers, letters): large muted mono or sans, top-left.

**Type**
- Mono (IBM Plex Mono or ui-monospace) for values, math, variables, token names and step labels. Sans (Inter or Helvetica) in caps, letter-spaced 0.1–0.14 em, for part names, captions, slot labels and notes.
- Sizes in viewBox units: dims 17, direct labels 19, captions and notes 16, in-object slot labels 13.

**Page**
- One self-contained HTML file, one `<svg>` per figure, `viewBox` 1440 wide (height to fit), `width: 100%`, rounded 12 px, ~32 px gap between figures.
- Light: gray ground, white object fill. Dark: near-black ground with an object fill darker than the ground, light ink. Define `--bg --surface --mid --ink --muted --faint --on-ink` on `:root`, redefine under `prefers-color-scheme: dark` guarded by `:root:not([data-theme="light"])`, and again under `:root[data-theme="dark"]`. No texture, grain or paper effect. Nothing else is colored.
- Kit (copy from `examples/card-diagram.html`): `begin(id)` (hatch), `rect`, `line`, `text`, `dim(x1,y1,x2,y2,label,{side,ext,ticks})`, `label(px,py,lx,ly,name,side)` direct label, `nb(x,y,n)` boxed note number, `notes(y,[…])`, `partTag(px,py,cx,cy,n,name)` circled part number, and for oblique stacks `OB` (matrix + origin), `layer(z)` (a projected group) and `P(x,y,z)` (screen point for projection lines and label dots). Avoid global names that clash with `window` (`top`, `name`, `status`).
- Before delivering: render both themes, check no label touches a line or shape, leaders don't cross, every boxed number has a note and every note has a number, and the subject has even empty space around it rather than at one edge.

Reply in one sentence: what was drawn, plus any [TBD] value.

---

# Spec mode

Draw one subject the way Apple's accessory dimensional drawings do (developer.apple.com/accessories/dimensional-drawings): everything on one sheet, notes top-left, views spread across the sheet with smaller detail views, tables beside the views that use them. Output is one self-contained HTML file (on claude.ai, publish it as an Artifact). Worked examples: `examples/dialog-sheet.html` and `examples/card-sheet.html` next to this file — copy their drawing kit instead of starting from zero.

## House rules

1. **Monochrome.** Black line on white paper, grays only. No accent color anywhere. Tell things apart by line weight, dash pattern, hatch and light gray fill. Single light theme by design (`color-scheme: light`, every color explicit).
2. **Architecture, not content.** For UI, draw slots, never real copy: lead box with an X, `TITLE SLOT · FLEX`, `TRAIL` / `ACTION` (hug), `CONTENT SLOT · FILL`, `META SLOT · FLEX · WRAPS`, `LABEL SLOT`, `VALUE SLOT`. No text-width dimensions. Size words: HUG (sized by content), FLEX (takes remaining space), FILL (parent width).
3. **Variables, not numbers.** Every dimension on the drawing is a code; tables give code, variable name, value. Derived values are formulas with the result: `H1 header-min = 2·S1+H3 = 64`, `R2 r-inner = R1−B−S6 = 11`, `E1 = BL+SP−Y`, `E2 = BL+SP`, `E3 = BL+SP+Y`.
4. **Unbranded.** No product or company names, logos or wordmarks in the drawing, title block or footer unless the user asks. Use neutral names (`UI-CARD-001`, "UI component reference").
5. **Never invent values.** Anything not in the source is `[TBD]` (e.g. viewport margin, overflow behavior, unpublished physical dims) and is drawn as a proportional schematic.

## Code system

| Prefix | Meaning | Examples |
|---|---|---|
| S# | spacing | S1 container pad, S2 inner pad, S3 stack gap, S4 min gap flex↔hug, S5 lead gap, S6 inner inset |
| H# / Z# / I# | heights / square sizes / icon size | H1 header-min, H2 footer-h, H3 action-h, H5 input-h, Z1 lead, I1 icon |
| B, W | border, width (range or fixed) | W = 420–680 |
| R# | radius | R1 container, R2 inner (derived), R3 control, R4 pill = h ÷ 2 |
| Y, BL, SP, E# | shadow offset/blur/spread, envelope | E1 top, E2 side, E3 bottom |
| T1, V1, MH | touch min, viewport margin, max height | MH = 100vh − 2·V1 |

## Page structure

1. **Title block (HTML, top).** Styled like the drawing: 1.2 px black frame, radius 16, 0.8 px black cell rules, plain caps lettering (Arimo/Helvetica), 9.5 px labels, caps values. Cells: big title (light caps, letter-spaced) + one-line scope, DWG NO., REV, SHEETS, UNITS, SCALE, SOURCE. Then `NOTES: (UNLESS OTHERWISE SPECIFIED)` general notes in two aligned columns (do not scale; units; slots are placeholders; codes and formulas; HUG/FLEX/FILL; [TBD]; grayscale). Then a row of 6–7 key variables (`W · card-w 420–680`, `H1 · header-min 2·S1 + H3`…), formulas in lowercase.
2. **Drawing sheet(s).** One SVG per sheet, `viewBox="0 0 1700 1060"`, `width:100%; min-width:1100px` inside an `overflow-x:auto` frame. Rounded sheet border. No title block on the sheet (it costs space); identity goes in a footer line under the sheet: `DATE | UI COMPONENT REFERENCE` left, `NAME, ARCHITECTURE | n` right.
3. Prefer **one sheet**. Add a sheet only when one can't hold it legibly; every extra sheet keeps the identical structure, and notes cross-reference (`SEE SHEET 2`).

## Sheet layout (one sheet)

- **Notes** top-left: boxed numbers `[1]` that also appear on the drawing next to the feature; un-numbered lines for general rules.
- **Plan view** top-center at 0.9–1.1:1: wireframe with slots; ordinate/chain dims top and bottom (codes, FLEX/HUG words, overall W with range label), region chain left (H1 / HUG / H2, overall `(REF n)`), inner chain right (S1 / H3 / S1 / S2 / S3 …), radius callouts, dash-dot centerlines, boxed note numbers, and dashed detail circles with labels (`DETAIL C`) pointing at features drawn larger elsewhere.
- **Under the notes:** shadow envelope (card outline, hatched footprint, dash-dot envelope, E1/E2/E3).
- **Left/bottom-left:** exploded axonometric of the layer stack with labels and leaders on the left (canvas → [scrim] → shadow → container → inset surfaces → slots & controls).
- **Bottom row:** nested-corner detail (shared center, B and S6, `R2 = R1 − B − S6`), radius details (one quarter-arc per radius, 2–2.4:1), inner/body detail at ~0.65:1, plus component-specific details.
- **Right column:** variable tables (Spacing, Size, Radius, Shadow, plus Viewport / Keep-out when relevant).

## Component-specific views (pick what applies)

- Card / list row: body well insets, stack gap, flex vs hug footer, touch target.
- Dialog / sheet / popover: viewport placement (centered on both axes, full-bleed scrim, MH box, V1 [TBD], envelope inside the viewport), scrim layer in the exploded view, header controls (lead R4, close Z2 with centered icon `(Z2−I1)÷2`), field detail (label → S4 → input H5, pad S9, R3), footer actions (end-aligned, secondary pill, gap S7, primary R3, per-button pad).
- Button / chip / input: detail at 2–4:1, states side by side, hit area vs drawn height.
- Navigation / page layout: plan per breakpoint, column grid, safe areas.
- **Physical products:** third-angle top / front / right views + section A–A (hatched cut solids), a positions sheet area (packed, deployed, parts removed) and a size-range strip to scale; units mm; published specs only, sources listed, conflicts flagged `CONFLICT`, everything else [TBD]; no logos or surface textures.

## Measuring a UI component

Read the real source (repo file, Storybook story, design-canvas file, artifact, URL). Strip zoom and template holes, render in Playwright, wait for fonts, walk the box tree (`getBoundingClientRect` + `getComputedStyle`: padding, gap, radius, border, min-height, flex-shrink, min-width). Survey every instance with grep counts so the variables reflect the system, not one example.

## Drawing kit (SVG, inline JS)

Use one `begin(svgId)` per sheet that creates a `<defs>` with a unique arrow marker and a 45° hatch pattern id, and draws the sheet border. Helpers, all in sheet units:

```js
dimH(x1,x2,y,ext1Y,ext2Y,label,{below,dx,dy,c})   // horizontal dim; <28px span → arrows outside
dimV(y1,y2,x,ext1X,ext2X,label,{side:'l'|'r'})    // vertical dim; no side → rotated label
callout(px,py,ex,ey,label,dir)                    // dot + leader + shoulder + text
rad(cornerX,cornerY,r,'tl'|'tr'|'bl'|'br',label,len)
nb(x,y,n)                                          // boxed note number
detailRef(cx,cy,r,label,lx,ly)                     // dashed circle + leader
table(x,y,title,[[code,variable,value]…])          // widths 40/88/96, returns next y
notes([[n,text]…])                                 // n=0 → unnumbered
```

Conventions: object outline 1.6 px; dim lines 0.7 px; extension lines 0.55 px, 3 px off the feature, 5 px overshoot; 7 px filled arrows; dim text 11 px with a white `paint-order: stroke` knockout; slot labels 9 px caps letter-spaced; view titles 12 px caps letter-spaced with a 9.5 px subtitle (`SCALE 0.9:1 · …`). Slots dashed 4 3; hug elements light gray fill; inset surfaces lighter gray; phantom/region lines long-dash; envelopes dash-dot; keep-outs hatched.

Pitfalls: a CSS `fill` rule on a class overrides a `fill="url(#hatch)"` attribute, so hatched shapes need a class without `fill`. Inside transformed groups (exploded view) use `vector-effect: non-scaling-stroke`, and set CSS-variable fills via `style`, not attributes. Stagger labels in tight chains with `dx`/`dy`. Exploded projection: `matrix(.94s, .17s, −.34s, .47s, tx, ty−z)`.

## Before publishing

- Every code on the drawing exists in a table; every formula closes (header = 2·S1 + H3, overall = sum of regions).
- No brand text; no color (grep for hex values outside the gray tokens); no content-dependent widths.
- Render once with Playwright at 1800 px wide, look at the screenshot for overlapping labels and elements crossing the sheet border, fix in one pass, then deliver (publish as an Artifact on claude.ai, or save the .html).

To animate the finished sheet, use the `diagram-motion` skill.

Reply in one or two sentences: what was drawn, and any [TBD] values the team should define.
