# SemiPaint24 — Help Guide

---

## Table of Contents

1. [Understanding SG24](#1-understanding-sg24)
2. [The Interface](#2-the-interface)
3. [Working with Colors](#3-working-with-colors)
4. [Drawing Tools](#4-drawing-tools)
5. [The Grid System](#5-the-grid-system)
6. [Converting Images](#6-converting-images)
7. [Saving and Exporting](#7-saving-and-exporting)
8. [Loading on Real Hardware](#8-loading-on-real-hardware)
9. [Keyboard Shortcuts](#9-keyboard-shortcuts)
10. [FAQ](#10-faq)

---

## 1. Understanding SG24

### What Makes SG24 Special

The TRS-80 Color Computer's MC6847 Video Display Generator supports a little-known extended mode called **Semigraphics 24**. Instead of the coarse 4×6 blocks of SG4 or the 4×3 blocks of SG6, SG24 divides each character position into **24 individually controllable dot-boxes**, each just **2 pixels wide by 1 pixel tall**.

This gives an effective resolution of **64 × 192 pixels** — remarkably fine for a machine of its era — while keeping memory usage at a compact **6,144 bytes**.

### The Cell Model

The screen is a grid of **32 columns × 192 rows** of cells. Each cell is exactly 2 display pixels wide:

```
┌─────┬─────┐
│  L  │  R  │  ← One cell: Left pixel and Right pixel
└─────┴─────┘
```

Both pixels in a cell share **one color** chosen from 8 options. You can independently turn the left pixel on, the right pixel on, both on, or both off (black).

### The 8 Colors

SG24 uses the MC6847's CSS=0 color set. **Black is not a color** — it's the absence of illumination. The 8 actual colors are:

| Index | Color | Appearance |
|---|---|---|
| 0 | **Green** | Bright phosphor green |
| 1 | **Yellow** | Warm yellow |
| 2 | **Blue** | Deep blue |
| 3 | **Red** | Saturated red |
| 4 | **Buff** | Near-white (full luminance) |
| 5 | **Cyan** | Blue-green |
| 6 | **Magenta** | Pink-purple |
| 7 | **Orange** | Amber orange |

### The Byte Format

Each cell is stored as one byte in screen memory at `$0400–$1BFF`:

```
 D7  D6  D5  D4  D3  D2  D1  D0
  1  C2  C1  C0   L   R   x   x
  │   └───┬───┘   │   │   └───┴── Don't care (hardware writes 1)
  │   Color 0–7   │   └────────── Right pixel ON
  └─ SG luma flag └────────────── Left pixel ON
```

Black cells are stored as `$20` (ASCII space) or `$80` (SG byte with both off).

---

## 2. The Interface

### Menubar
The top bar contains five menus. The right side of the menubar shows the current **Tool**, **Zoom level**, and **Aspect ratio**.

### Sidebar (Accordions)
The left sidebar has four collapsible panels. Click any panel header to expand or collapse it:

- **Information** — live readout of canvas specs, zoom, aspect, grid status
- **Tools** — all drawing tools as icon buttons
- **Active Colors** — shows current Foreground (FG) and Background (BG) swatches with a swap button
- **SG24 Palette** — the full color selection panel

### Canvas
The main drawing area. The canvas is 64×192 pixels internally. It can be zoomed from 1× to 16× and displayed in 4:3 CoCo aspect ratio or square pixels.

### Bottom Bar
Zoom out / zoom in buttons and current zoom level. Hover over the canvas for cell information shown as a tooltip.

---

## 3. Working with Colors

### Selecting Colors

The **SG24 Palette** panel shows 8 colors in a 2-column grid, plus Black at the bottom. Each color entry has **three sub-swatches**:

```
┌─────────────────────┐
│       GREEN         │
│ ████ │ ██▌│ ▐██ │  │
│ Both │Left│Right│  │
└─────────────────────┘
```

- **Both** — both left and right pixels of the cell are on (full solid color)
- **Left** — only the left pixel is on; right is black
- **Right** — only the right pixel is on; left is black

**Click** a sub-swatch to set it as the **Foreground (FG)** color.  
**Right-click** (or **Shift+click**) a sub-swatch to set it as the **Background (BG)** color.

The **Black** entry (spanning the full width at the bottom) sets both pixels off — the eraser default.

### Foreground and Background

The **Active Colors** panel shows the current FG (top) and BG (bottom) swatches. The swatch visually shows the color and sub-mode:
- Solid color = Both ON
- Left-half color = Left Only
- Right-half color = Right Only
- All black = Black (both off)

Click **⇄** to swap FG and BG.

When drawing, **left-click** uses the Foreground color. **Right-click** uses the Background color.

### Using the Eraser

The **Eraser** tool paints with the **Background color**. To erase to black:
1. Set the Background color to **Black** (click the Black entry in the palette with right-click or Shift+click)
2. Select the Eraser tool (or press `E`)
3. Draw over areas to erase

To "erase" with a different color (paint over), set the Background to that color first.

---

## 4. Drawing Tools

### Pencil `P`
Freehand drawing. Click and drag. Draws in a continuous line using Bresenham's algorithm for smooth diagonal strokes.

**Tip:** At zoom 4× or higher with the grid on, each cell is clearly visible, making pixel-precise work easy.

### Flood Fill `F`
Fills a contiguous region of cells that share the same color and sub-mode. Click anywhere in a region to fill it.

The fill algorithm works at **cell level** — it treats each 2-pixel-wide cell as a unit, so left-only and right-only cells are considered different even if they share the same color index.

### Eyedropper `K`
Click any cell on the canvas to pick up its color and sub-mode as the Foreground. Right-click to pick up as Background.

### Line `L`
Click and drag to draw a straight line. Release to commit. The line uses Bresenham's algorithm and can be snapped to the grid if snap is enabled.

### Rectangle `R`
Click and drag to draw a rectangle outline. Release to commit.

### Filled Rectangle
Drag to draw a solid filled rectangle. Available via the tool grid (solid square icon).

### Circle `C`
Click the center, drag to set the radius, release to commit an ellipse outline. The aspect ratio is unconstrained so you can draw true ellipses.

### Filled Circle
Same as Circle but fills the interior.

### Eraser `E`
Paints with the current Background color. See [Using the Eraser](#using-the-eraser) above.

### Pan (Space)
Hold Space and drag to scroll the canvas when zoomed in beyond the window. Alternatively, hold Space to temporarily activate Pan mode from any other tool.

---

## 5. The Grid System

### Character Cell Grid
Shows the boundaries of each 2-pixel-wide cell. Default color: blue at low opacity. Visible from zoom 2× and above.

### Pixel Grid
Shows individual pixel boundaries. Visible from zoom 4× and above. Useful for precise Left/Right sub-mode work.

### Enabling the Grid
**View → Character Cell Grid** and **View → Pixel Grid** toggle each grid layer. A checkmark indicates it's on.

### Grid Settings `View → Grid Settings…`
Opens the Grid Settings dialog:

| Setting | Description |
|---|---|
| Character Cell Grid | Toggle cell grid on/off |
| Pixel Grid | Toggle pixel grid on/off |
| Snap to Grid | Snap drawing operations to grid boundaries |
| Grid Size (cells) | How many cells between grid lines (1 = every cell, 4 = every 4 cells) |
| Cell Grid Color | Color picker for the cell boundary lines |
| Pixel Grid Color | Color picker for the pixel boundary lines |
| Grid Opacity | Transparency of grid lines (0.03–0.60) |

### Snap to Grid
When enabled, all drawing operations snap start and end points to the nearest grid cell boundary. Useful for creating clean pixel art or aligned geometric shapes. Toggle via **View → Snap to Grid** or in Grid Settings.

---

## 6. Converting Images

### Opening the Converter
**File → Convert Image to SG24…** — opens a file browser. Select any PNG, BMP, JPG, or GIF. The Convert dialog opens automatically.

Alternatively, **Filters → Convert Image to SG24…** does the same.

### The Convert Dialog

The dialog shows:
- **Left**: Original source image (scaled for display)
- **Right**: SG24 Preview at 2× zoom in CoCo 4:3 aspect ratio

The preview updates live as you adjust sliders.

### Dithering Algorithms

| Algorithm | Best For |
|---|---|
| **Atkinson** | Photographs, portraits — recommended starting point |
| **Floyd-Steinberg** | Smooth gradients and detailed images |
| **Ordered Bayer 2×2** | Retro-stylized look, predictable patterns |
| **Ordered Bayer 4×4** | More refined ordered pattern |
| **Checkerboard + Atkinson** | Images with large dark areas; forces alternating cells black |
| **Blue Noise** | Organic random-looking dither |
| **None** | Solid-color graphics, logos, limited-palette art |

### Conversion Controls

**Brightness** (0.2–2.0) — Lighten or darken the source before conversion. Try 1.2–1.4 for photos that look dark on the CoCo's NTSC output.

**Contrast** (0.2–3.0) — Expand or compress the tonal range. Higher contrast often produces crisper results on the limited SG24 palette.

**Saturation** (0.0–3.0) — Boost color vividness. The 8 SG24 colors are fully saturated, so reducing to 0.5–0.8 can produce more natural-looking results for photographs.

**Checker Strength** (0.0–1.0) — Pre-darkens alternating cells before dithering. This encourages the ditherer to use black for every other cell, producing a halftone-like effect. Set to 0 for normal full-color dithering. Effective range: 0.2–0.5.

### How the Conversion Works

The converter operates at **cell level** (32×192 cells, not 64×192 pixels):

1. Scale the source image to 64×192
2. Apply Brightness, Contrast, Saturation adjustments
3. Apply Checker Strength pre-darkening to alternating cells
4. For each cell, average the two source pixels to get a representative color
5. Add accumulated dithering error from neighbors
6. Compare the adjusted color to all 8 SG24 colors and to black
7. If the nearest color wins over black → paint the cell that color
8. Decide left/right/full based on individual pixel brightness (if one half is ≤35% of average, it stays black)
9. Propagate the quantization error to neighboring cells

This cell-level approach prevents the "half-pixel checkerboard" artifact that occurs when individual pixels are forced to left-only or right-only sub-modes.

### Tips for Best Results

- For **portraits**: Atkinson dithering + Brightness 1.1 + Saturation 0.7
- For **high-contrast artwork**: Floyd-Steinberg + Contrast 1.5
- For **retro game style**: Ordered Bayer 4×4 + Checker Strength 0.3
- For **text/logos**: None (no dithering) + Contrast 2.0
- The **Preview button** lets you see the result before committing; click **Convert** to apply to the canvas

---

## 7. Saving and Exporting

### Save .S24 File
`File → Save .S24 File` — saves the raw 6,144-byte screen memory dump. This file can be:
- POKEd directly to `$0400` in a CoCo emulator or on hardware
- Loaded by the exported BASIC or assembly program
- Opened again in SemiPaint24

### Export CoCo BASIC (.BAS)
`File → Export CoCo BASIC (.BAS)` — generates a self-contained CoCo Extended BASIC program that activates SG24 mode and displays your picture.

The program uses **hex-string data packing**: each `DATA` line holds 32 bytes as a 64-character hex string. This produces only 192 data lines (vs. 600+ with decimal values), fitting comfortably in the CoCo's available RAM.

**To run on a CoCo:**
1. Load the `.BAS` file (cassette, disk, or DriveWire)
2. Type `RUN`
3. The program sets up SG24 mode and displays the picture
4. The program loops at `GOTO 110` — press Reset to return to BASIC

### Export Assembly Source
`File → Export Assembly Source` — generates LWASM-compatible 6809 assembly with the picture data embedded as `fcb` directives. Suitable for inclusion in larger CoCo assembly programs.

### Export PNG / BMP
Export a scaled rendering of the current canvas at 4× (PNG) or 2× (BMP) resolution, with the CoCo NTSC display colors. Useful for documentation, sharing, and embedding in web pages.

---

## 8. Loading on Real Hardware

### Method 1: BASIC Program
1. Save your picture as a BASIC program (`.BAS`)
2. Transfer to CoCo via cassette tape, floppy disk, or DriveWire
3. `CLOAD` (cassette) or `LOAD"filename"` (disk) then `RUN`

### Method 2: Direct POKE
From Extended BASIC:
```basic
10 ' Load SG24 image from DATA
20 PCLEAR4:PMODE4,1:SCREEN1,0
30 POKE 359,57
40 POKE 65314, PEEK(65314) AND &HF
50 POKE &HFF9C, &HA
60 FOR I=0 TO 6143: READ D: POKE 1024+I,D: NEXT I
70 GOTO 70
```
Then add DATA lines with your raw byte values.

### Method 3: Assembly / Machine Code
Use the exported `.asm` file with LWASM:
```
lwasm --format=ihex sg24.asm -o sg24.hex
```
Load the resulting binary at `$3E00` and `EXEC &H3E00`.

### Emulator Notes
- **MAME CoCo driver**: Load `.S24` file into RAM at `$0400` using the debugger
- **XRoar**: Use the memory viewer to paste raw bytes at `$0400`; set VDG mode via PIA
- **VCC**: Full CoCo 1/2/3 emulation; run the BASIC program directly

---

## 9. Keyboard Shortcuts

| Key | Action |
|---|---|
| **Drawing Tools** | |
| `P` | Pencil |
| `F` | Flood Fill |
| `K` | Eyedropper (pick color) |
| `L` | Line |
| `R` | Rectangle |
| `C` | Circle |
| `E` | Eraser |
| `Space` (hold) | Pan / scroll canvas |
| **Edit** | |
| `Ctrl+Z` | Undo |
| `Ctrl+Y` | Redo |
| **Zoom** | |
| `+` or `=` | Zoom in |
| `-` | Zoom out |
| Mouse Wheel | Zoom in/out at cursor |
| **Drawing Modifiers** | |
| Right-click | Draw with Background color |
| Shift+click palette | Set Background color |

---

## 10. FAQ

**Q: Why does my image look like a noisy checkerboard after conversion?**  
A: This usually means the Checker Strength is too high, or you are using the Checkerboard + Atkinson mode with a bright image. Set Checker Strength to 0 and try Atkinson dithering.

**Q: I got an OM Error when running the BASIC program.**  
A: This was a bug in versions before v4.0 that has been fixed. The new `.BAS` export uses hex-string packing which is 3× more compact. Re-export your image with the current version.

**Q: The colors look different on my real CoCo vs the editor.**  
A: The editor uses accurate NTSC phosphor values from the MC6847 datasheet, but real CoCo colors depend heavily on your TV/monitor's settings, the composite video output quality, and RF vs direct video. Try adjusting brightness and contrast in the conversion settings to compensate.

**Q: Can I load a .S24 file from a different SG24 editor?**  
A: Yes. Any standard SG24 file (6,144 bytes, sequential scan lines, standard byte format) will load correctly. Files created by ROBCFG's SG24 Editor and similar tools are fully compatible.

**Q: The grid isn't showing up even when I turn it on.**  
A: The pixel grid only appears at zoom 4× and above. The character cell grid appears at zoom 2×. If it's still not visible, check the Grid Settings (View → Grid Settings) and increase the opacity.

**Q: Why is there no black in the color palette?**  
A: Black is not one of the 8 MC6847 SG24 colors. It's the *absence* of illumination — both pixels in a cell are simply off. The separate **Black** entry at the bottom of the palette sets the sub-mode to "both off", stored as ASCII space `$20` in screen memory.

**Q: Can I save my work and come back to it?**  
A: Use **File → Save .S24 File** to save the raw image. You can re-open it with **File → Open .S24 File**. Full project save with undo history is planned (see BACKLOG.md, item E-01).

**Q: What CoCo models support SG24?**  
A: SG24 works on the CoCo 1, CoCo 2, and CoCo 3. The mode uses the MC6847 VDG (standard on CoCo 1/2) or the MC6847T1 variant on later models. CoCo 3 can also display SG24 via its MC6847 compatibility mode, though the GIME chip offers higher-resolution modes.

**Q: What is the "don't care" bits warning in the byte format?**  
A: Bits D1 and D0 of each SG24 byte are ignored by the MC6847 hardware — any value works. Real hardware files (like those from ROBCFG's editor) typically set these to `1`, giving values like `$8F` instead of `$8C` for Green-both. SemiPaint24 writes these bits as `1` (`| 0x03`) to match real hardware files.
