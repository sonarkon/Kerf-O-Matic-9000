# Kerf-O-Matic 9000

A browser tool for laser-cut kerf bending (living hinge) patterns, made for **cylinders and pendant lampshades**. Everything is metric and runs in a single HTML file – no install, no build step, nothing is uploaded.

**Live:** https://sonarkon.github.io/Kerf-O-Matic-9000/

![Creative mode with bands and a wave modulator](kerf-o-matic-2d.jpg)

## Contents

- [Quick start](#quick-start)
- [Modes](#modes) – Simple · Creative · Test Pattern · Lamp
- [Board and cylinder](#board-and-cylinder)
- [Preview](#preview)
- [Bendability estimate](#bendability-estimate)
- [Export and files](#export-and-files)
- [Tips for acrylic](#tips-for-acrylic)
- [Run locally](#run-locally) · [Credits](#credits) · [License](#license)

## Quick start

1. Enter the **width (= circumference)** and **height** of the board – or the diameter you want, and the width is calculated.
2. Choose the **material** and its **thickness**, then a **shape** and the **slot** values.
3. Check the **bendability** on the right and the preview (try *3D* and the lamp button).
4. Cut a few strips in **Test Pattern** mode to find the right values for your material.
5. **Save SVG** and cut. For a lamp, switch to **Lamp** and save the complete cut set.

## Modes

| Mode | What it is for |
|---|---|
| **Simple** | One even pattern across the board. |
| **Creative** | Bands and modulators for lampshade designs on the same slot grid. |
| **Test Pattern** | A set of labelled test strips that vary one value. |
| **Lamp** | The parts for a pendant lamp (E27) around the Simple/Creative shade. |

### Simple

- **Shapes:** pill (one closed path with true arcs), **dogbone** (round relief holes at the slot ends), **S-slot** (constant width, exact arcs), line, meander, ellipse, rectangle, diamond, hexagon, chevron, cross.
- **Slots:** length, width, vertical and horizontal gap.
- **Vertical offset:** top-aligned, centered (top and bottom edges mirror each other) or a custom value.

### Creative

Everything from Simple, plus per-slot variation on the same grid, so the shade still bends evenly:

- **Bands** – horizontal zones from top to bottom, each with its own shape and optional slot size (mix pill, dogbone, S-slot …).
- **Modulators** – a source drives a slot property:
  - sources: linear gradient (any angle, optionally mirrored), radial, waves, noise, or a greyscale **image**;
  - targets: slot width, slot length, remove slots, swap the shape;
  - they stack from top to bottom with adjustable strength.
- With **Seamless** on, radial, waves and noise wrap around the cylinder, so the design continues across the glue seam.
- An image is stored (downscaled) with the design, so it reloads completely.

### Test Pattern

- Strips that vary one value (horizontal gap, slot length, vertical gap or slot width) from *From* to *To*; *Count* can also be 1.
- Each strip is engraved with the varied value, all slot values (`Pill L 20 W 2 Gap V 4 H 2`) and the project name – lines shrink to fit, and wrap on short strips.
- The panel lists the smallest inner diameter per strip that should stay within the material's “good” range.

### Lamp

![Lamp mode: the assembled pendant in 3D, exploded, with the light on](kerf-o-matic-lamp.jpg)

Parts for a **pendant lamp with an E27 socket** around the shade from Simple or Creative (whichever was used last):

- **Top / bottom part:** spider with socket hole, disc with socket hole, disc, ring, or none. The socket hole defaults to Ø 40 mm (E27 shade ring – check yours, 40–42 mm is common).
- **Adjustable:** spokes, spoke and hub width, ring width, fit clearance; kerf compensation applies.
- **Attachment:**
  - **Glued in** – the parts sit inside the cylinder.
  - **Tabs & slots** – the shade gets tabs on its top and bottom edges (edge slots under a tab are left out); the parts sit on the ends with matching slots. The tabs also appear in the Simple/Creative shade file.
  - **Edge interlock** – no extra tabs: the parts get curved slots on the wall's exact curvature for the wall pieces between the open edge slots and reach through those slots to an outer rim. Needs open edges whose slots are at least one material thickness deep.
- **2D view and SVG** contain the complete cut set – the flat shade plus the parts, engraved TOP/BOTTOM.
- **3D view** shows the assembled lamp with the light at the socket; the **Explode** slider lifts the parts off.
- **STL/OBJ** export the whole assembly.

## Board and cylinder

- **Cylinder sizing:** enter the inner, mid-plane or outer diameter; the width (circumference) is calculated on the material's mid-plane.
- **Seamless:** adjusts the horizontal gap so an even number of columns fills the circumference – the pattern continues across the glue seam.
- **Seam joint:** a glue tab past the circumference, or finger joints (fingers on one edge, matching notches on the other).
- **Open edges:** slots run out through the top and bottom edges, so no continuous solid strip is left.
- **Kerf compensation:** slots are drawn smaller and the outline larger by half the laser kerf, so the cut part has the nominal size.
- **Cut order:** zigzag, or every other column first (less heat build-up – useful for acrylic).
- Side margins, optional outer border, 90° rotated output.

## Preview

- **2D / 3D** switch at the top left.
- At the top right:
  - **Bend** (Simple/Creative) – from flat to a closed cylinder; also used for the bendability estimate and the STL/OBJ export.
  - **Explode** (Lamp, 3D) – lifts the lamp parts off the shade.
  - **Lamp** – glowing slots on a dark background, in 2D and 3D.
  - **Flex** – strain heatmap per slot: good / borderline / too high / rigid.
- **3D** shows the extruded part with its real thickness – the same closed solid as the STL/OBJ export. Acrylic and PETG are semi-transparent, wood is opaque. Drag to orbit, right-drag to pan, wheel to zoom. three.js is loaded only when 3D is opened.

![3D preview: closed cylinder with the light on](kerf-o-matic-3d.jpg)

## Bendability estimate

Strain in the twisting bridges between the slot columns, computed for every slot-to-slot link and scaled with **Bend** (100 % = closed cylinder, 50 % = half the curvature). The panel shows the maximum strain and the smallest inner diameter that stays “good” for the chosen material (acrylic, PETG, plywood, MDF).

It is a rough guide based on Saint-Venant torsion and does not replace a test cut.

## Export and files

The **Save** button exports in the format chosen from its menu:

| Format | Content |
|---|---|
| **SVG** | The cut file for the laser (true size in mm). |
| **STL** / **OBJ** | A closed 3D solid with real thickness, bent as set with *Bend* (Lamp: the whole assembly). Zero-width cuts get a 0.2 mm kerf; OBJ has welded vertices for Blender. |
| **PNG** | A seamless, square 2048 px mask tile of the base pattern (white = material, black = hole); the tile size in mm is in the file name. |

**All parameters are saved with the design** – in the file name, as a *Parameters* layer and as metadata. **Load** restores a design from a saved SVG, OBJ or PNG (STL cannot hold the parameters). An optional **project name** becomes the file-name prefix and the first line of the parameter layer.

### SVG layers

| Layer | Colour | Use |
|---|---|---|
| Cuts | red | cut |
| Outer border | red | cut |
| Engrave | black | engrave (labels on test strips and lamp parts) |
| Parameters | blue | information only – turn its output off in your laser software, or save with the layer hidden |

## Tips for acrylic

- Cast acrylic (GS) tolerates more bending than extruded (XT).
- Bending with a heat gun or in an oven (~90 °C) greatly reduces the residual stress.
- Round slot ends (pill, dogbone) are less prone to cracking than pointed ones.
- Use **LED bulbs** in acrylic lamps – acrylic softens at about 80 °C.
- Cut a few test strips first; the bendability estimate is only a guide.

## Run locally

Open `index.html` in any modern browser. The 3D view and the STL/OBJ export load small libraries from a CDN on first use; everything else works offline.

## Credits

Inspired by [KerfSmith](https://kerfsmith.com/web-app.html).

## License

MIT – see [LICENSE](LICENSE).
