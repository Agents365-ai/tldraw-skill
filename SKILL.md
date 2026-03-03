---
name: tldraw-skill
description: Use when user requests diagrams, flowcharts, architecture charts, or visualizations. Also use proactively when explaining systems with 3+ components, complex data flows, or relationships that benefit from visual representation. Generates .tldr files and exports to PNG/SVG locally using tldraw-cli.
---

# tldraw Diagram Skill

## Overview

Generate modern whiteboard-style diagrams as `.tldr` JSON files and export to PNG/SVG using `tldraw-cli`. tldraw produces clean hand-drawn aesthetic diagrams with rich shape libraries and smooth arrow routing.

**Format:** `.tldr` JSON
**Export:** PNG, SVG (via `@kitschpatrol/tldraw-cli`)
**Aesthetic:** Hand-drawn whiteboard style

## When to Use

**Explicit triggers:**
- "draw a diagram", "create a chart", "visualize this", "make a flowchart"
- "tldraw diagram", "whiteboard diagram"

**Proactive triggers:**
- Explaining a system with 3+ components
- Describing data flows or pipelines
- Showing relationships between services/modules
- Architecture overviews, sequence flows, decision trees

## Prerequisites

```bash
# Install tldraw-cli
npm install -g @kitschpatrol/tldraw-cli

# Verify
tldraw --version
```

Works identically on macOS, Windows, and Linux — no extra setup required.

## Workflow (5 Steps)

### Step 1: Check Dependencies
```bash
tldraw --version
```
If not found: `npm install -g @kitschpatrol/tldraw-cli`

### Step 2: Plan the Diagram
- Identify all nodes and their types (geo shapes)
- Sketch a grid layout: ~200px per node horizontally, ~150px vertically
- Assign x/y coordinates before writing JSON
- List all connections (arrows) with source and target shape IDs

### Step 3: Generate .tldr File
Write the JSON file following the skeleton and rules below.

### Step 4: Export to Image
```bash
# Output goes to the specified directory as <filename>.png
tldraw export diagram.tldr -f png --scale 2 -o ./
```

### Step 5: Report to User
Tell the user:
- Path to the `.tldr` source file
- Path to the exported PNG/SVG
- Brief description of what was generated

---

## File Format

### Complete .tldr Skeleton

```json
{
  "tldrawFileFormatVersion": 1,
  "schema": {
    "schemaVersion": 1,
    "storeVersion": 4,
    "recordVersions": {
      "asset": {"version": 1, "subTypeKey": "type", "subTypeVersions": {"image": 2, "video": 2, "bookmark": 0}},
      "camera": {"version": 1},
      "document": {"version": 2},
      "instance": {"version": 17},
      "instance_page_state": {"version": 3},
      "page": {"version": 1},
      "shape": {"version": 3, "subTypeKey": "type", "subTypeVersions": {"group": 0, "embed": 4, "bookmark": 1, "image": 2, "text": 1, "draw": 1, "geo": 7, "line": 0, "note": 4, "frame": 0, "arrow": 1, "highlight": 0, "video": 1}},
      "instance_presence": {"version": 4},
      "pointer": {"version": 1}
    }
  },
  "records": [
    {"id": "document:document", "typeName": "document", "gridSize": 10, "name": "", "meta": {}},
    {"id": "page:page1", "typeName": "page", "name": "Page 1", "index": "a1", "meta": {}}
    /* shapes and arrows go here */
  ]
}
```

**Critical rules:**
- `document:document` and `page:page1` records are ALWAYS required
- All shapes go in the `records` array after the page record
- All shapes have `"parentId": "page:page1"`
- Shape IDs use format `"shape:xxx"` with unique suffix (e.g., `"shape:s1"`, `"shape:a1"`)
- `index` values MUST start with `"a"` followed by digits or uppercase letters: `"a1"`, `"a2"`, ..., `"a9"`, `"aA"`, `"aB"`, ..., `"aZ"`, `"a10"`, etc.
- **Never use `"b1"`, `"c1"` etc. as indices** — only `"a*"` format is valid for shapes

---

## Geo Shape Record

```json
{
  "id": "shape:s1",
  "typeName": "shape",
  "type": "geo",
  "parentId": "page:page1",
  "index": "a1",
  "x": 100,
  "y": 100,
  "rotation": 0,
  "isLocked": false,
  "opacity": 1,
  "meta": {},
  "props": {
    "w": 180,
    "h": 60,
    "geo": "rectangle",
    "color": "blue",
    "labelColor": "black",
    "fill": "semi",
    "dash": "draw",
    "size": "m",
    "font": "draw",
    "text": "API Gateway",
    "align": "middle",
    "verticalAlign": "middle",
    "growY": 0,
    "url": ""
  }
}
```

### Geo Types

| `geo` value | Use for |
|-------------|---------|
| `rectangle` | services, modules, components |
| `ellipse` | databases, start/end nodes |
| `diamond` | decision points |
| `cloud` | external services, infrastructure |
| `hexagon` | event hubs, message buses |
| `triangle` | gateways, load balancers |
| `star` | highlights, key features |

### Color Palette

| `color` | Use for |
|---------|---------|
| `blue` | clients, core services |
| `green` | success, databases, storage |
| `orange` | queues, event buses, warnings |
| `red` | external APIs, errors, alerts |
| `light-red` | soft alerts, secondary warnings |
| `violet` | gateways, security, auth |
| `yellow` | decisions, caches |
| `grey` | neutral, background, legacy |
| `light-blue` | secondary services, metadata |
| `black` | titles, emphasis |

### Style Options

| Property | Values | Notes |
|----------|--------|-------|
| `fill` | `semi`, `solid`, `none`, `pattern` | `semi` = tinted fill (recommended) |
| `dash` | `draw`, `solid`, `dashed`, `dotted` | `draw` = hand-drawn default |
| `size` | `s`, `m`, `l`, `xl` | `m` = default |
| `font` | `draw`, `sans`, `serif`, `mono` | `draw` = default whiteboard style |

---

## Arrow Record

```json
{
  "id": "shape:a1",
  "typeName": "shape",
  "type": "arrow",
  "parentId": "page:page1",
  "index": "aG",
  "x": 0,
  "y": 0,
  "rotation": 0,
  "isLocked": false,
  "opacity": 1,
  "meta": {},
  "props": {
    "dash": "draw",
    "size": "m",
    "fill": "none",
    "color": "black",
    "labelColor": "black",
    "bend": 0,
    "start": {
      "type": "binding",
      "boundShapeId": "shape:s1",
      "normalizedAnchor": {"x": 0.5, "y": 1},
      "isExact": false
    },
    "end": {
      "type": "binding",
      "boundShapeId": "shape:s2",
      "normalizedAnchor": {"x": 0.5, "y": 0},
      "isExact": false
    },
    "arrowheadStart": "none",
    "arrowheadEnd": "arrow",
    "text": "",
    "font": "draw"
  }
}
```

### Arrow Connection Rules

- Arrow record `x` and `y` are always `0, 0`
- Use `"type": "binding"` with `boundShapeId` to connect to a specific shape
- `normalizedAnchor` specifies WHERE on the target shape the arrow connects (0–1 range):
  - `{x: 0.5, y: 0}` = top center
  - `{x: 0.5, y: 1}` = bottom center
  - `{x: 0, y: 0.5}` = left center
  - `{x: 1, y: 0.5}` = right center
  - `{x: 0.5, y: 0.5}` = center
- Add `"text": "label"` in arrow props for labeled connections
- Use `"bend": 20` for slight curves to avoid overlap with other arrows

---

## Index Ordering Rules

Indices control z-order (stacking). Use this sequence:
```
a1, a2, a3, a4, a5, a6, a7, a8, a9,
aA, aB, aC, aD, aE, aF, aG, aH, aI, aJ, aK, aL, aM,
aN, aO, aP, aQ, aR, aS, aT, aU, aV, aW, aX, aY, aZ
```
- Geo shapes first: `a1` through `aF` (or as many as needed)
- Arrow shapes after: `aG`, `aH`, etc.
- Every shape must have a **unique** index

---

## Export Commands

```bash
# Check CLI version
tldraw --version

# PNG at 2x scale (recommended) — outputs diagram.png in ./
tldraw export diagram.tldr -f png --scale 2 -o ./

# SVG — outputs diagram.svg in ./
tldraw export diagram.tldr -f svg -o ./

# Transparent background
tldraw export diagram.tldr -f png --scale 2 --transparent -o ./

# Dark theme
tldraw export diagram.tldr -f png --scale 2 --dark -o ./
```

**Note:** `-o` is an output **directory**, not a file path. The output file is named after the input file.

---

## Layout Tips

- Allocate ~200px per node horizontally, ~150px vertically
- Plan a coordinate grid BEFORE writing the JSON to avoid overlaps
- For vertical flows (top-down): use columns spaced 240px apart
- For event bus pattern: place bus (hexagon) in a center row, services left/right with horizontal arrows
- Leave at least 40px margin around the canvas edges
- For wide shapes (like API Gateway spanning multiple services), set `w` to cover the full span

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `tldraw` command not found | Run `npm install -g @kitschpatrol/tldraw-cli` |
| `invalidRecords` on export | Check: index values must start with `a` (e.g., `a1`, `aA`) — never `b1`, `c1` |
| Blank/empty export | Verify `document:document` and `page:page1` records are present |
| Output file not found | `-o` is a directory; file name matches input: `tldraw export foo.tldr -o ./` → `./foo.png` |
| Arrow doesn't appear | Use `"type": "binding"` with `boundShapeId`; set arrow `x`/`y` to `0,0` |
| Shapes overlap | Plan a 200px grid before assigning x/y coordinates |
| Text not visible | Check `props.text` is set; if `fill: "none"`, ensure text color contrasts |
| Index collision | All shapes must have unique `index` values |
| Shape ID clash | Use unique IDs: `"shape:s1"`, `"shape:s2"`, `"shape:a1"`, etc. |
| Export fails | Ensure the `.tldr` file is valid JSON: `python3 -m json.tool file.tldr > /dev/null` |
