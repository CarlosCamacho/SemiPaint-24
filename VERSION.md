# SemiPaint24 — Version History

---

## v4.0 — Current Release
**Released:** 2025

### New Features
- **Accordion sidebar panels** — Information, Tools, Active Colors, and Palette panels are now collapsible, matching the CoCo 3 Paint program UI style
- **2-column palette layout** — Green/Yellow, Blue/Red, Buff/Cyan, Magenta/Orange, then Black full-width; each color has visible full/left/right sub-swatches
- **Larger palette swatches** — sub-cells are taller (22px+) and much easier to click
- **CoCo 4:3 aspect ratio** — View menu toggle between CoCo 4:3 display and square 1:1 pixels; status bar shows current aspect
- **Grid overhaul** — Grid Settings modal with character cell grid color, pixel grid color, opacity slider, grid size in cells, and snap to grid; grid is now visibly rendered (lineWidth fix)
- **Live conversion preview** — Convert modal auto-previews on slider change; preview renders at 2× in CoCo 4:3 aspect ratio

### Bug Fixes
- **Conversion checkerboard bug** — Completely rewrote conversion algorithm. Previous version forced every pixel to left-only or right-only, creating an ugly half-pixel checkerboard. New algorithm works at **cell level** (32×192), averages both source pixels per cell, picks best color, then decides full/half illumination from individual pixel brightness. Checker strength now correctly darkens entire cells, not individual pixels.
- **Checker Strength slider** — Was not affecting output. Now correctly pre-darkens alternating cells before dithering.
- **Grid not visible** — Grid lines were sub-pixel width and invisible at most zoom levels. Fixed with `1.5/z` lineWidth and minimum zoom thresholds.
- **BASIC export OM Error** — Previous export generated 614+ DATA lines with decimal values, overflowing CoCo BASIC's ~22 KB free RAM. New export uses **hex-string packing**: 192 DATA lines each containing 32 bytes as a 64-character hex string — 3× more compact, fits comfortably in RAM.
- **BASIC file extension** — Was `.txt`. Now correctly exports as `.BAS`.
- **Preview button** — Was not working. Preview now auto-runs on modal open and on any slider change.
- **"Apply to Canvas" renamed** to **"Convert"** for clarity.

### Removed
- **Filters menu** — Dithering and adjustment controls consolidated into the Convert Image modal
- **Status bar clutter** — Removed pixel/cell coordinate display from top-right (was redundant with bottom hover tooltip). Removed "64×192 SG24 — 32×192 cells — 8 colors" text from bottom bar (info lives in the accordion panel).
- **Cell Mode panel** — Removed separate panel; cell sub-mode is now selected directly by clicking the Full/Left/Right sub-swatch in the palette

### Changed
- **Eraser tool** now paints with the Background color/sub-mode (right-click on any tool also uses Background)
- **About dialog** updated with author name (Carlos Alberto Camacho), year, and MIT License notice

---

## v3.0
**Released:** 2025

### New Features
- **Light theme** — Switched from dark CRT-green theme to CoCo 3 Paint-style light Windows theme (gray panels, blue accents)
- **MC6847 color accuracy** — Display colors updated to accurate NTSC phosphor values per MC6847 datasheet Figure 21; Buff corrected to `#ffffff`
- **Correct color indexing** — Fixed critical bug where color index 0 was treated as Black; correct MC6847 order: 0=Green, 1=Yellow, 2=Blue, 3=Red, 4=Buff, 5=Cyan, 6=Magenta, 7=Orange. Black is the space character `$20`, not a palette entry.
- **S24 byte format confirmed** — Byte encoding verified against real `.S24` files: `0x80 | (color<<4) | (lOn<<3) | (rOn<<2) | 0x03`
- **`0x80` as valid black** — Loader now correctly handles both `$20` and `$80` as black cells (per MC6883 SAM datasheet Figure 17)
- **CT24.BAS technique** — BASIC export uses verified POKE 178 color tables from CT24.BAS (ROBCFG)
- **Assembly export** — Matches screen.asm style: `PSHS`, `LDD/STD` copy loop, `fcb` data at 32 bytes per line
- **View menu with aspect ratio** — CoCo 4:3 vs square pixel toggle
- **Grid Settings modal** — Cell/pixel grid, color, opacity, snap
- **Filters menu** — Separate modals for Dithering Method, Brightness/Contrast/Saturation, Checker Strength

### Bug Fixes
- **Left/right pixel storage model** — Rewrote internal pixel encoding to uniform bit1 model; both slots store element-on in bit1, eliminating the asymmetric old model that caused left/right-only painting to sometimes render as solid
- **Information panel** — Added accordion with live Zoom and Cell Mode display

---

## v2.0
**Released:** 2025

### New Features
- **Light theme** — CoCo 3 Paint-inspired gray/blue UI replacing the dark CRT theme
- **Correct SG24 byte format** — `1_C2C1C0_L_R_XX` confirmed from real `.S24` file analysis
- **Color index correction** — Green=0 (not Black=0) per MC6847 hardware
- **3-swatch palette** — Each color has Full/Left/Right sub-cells
- **Cell mode panel** — Both ON / Left Only / Right Only / All Black buttons
- **Zoom controls** — Mouse wheel, +/- keys, zoom buttons in bottom bar
- **Aspect ratio display** — CoCo 4:3 stretch
- **Character Cell Grid** — Blue cell-boundary overlay
- **Undo/Redo stack** — 50 levels
- **BASIC and Assembly export**
- **PNG and BMP export**

### Bug Fixes
- `pixelsToS24()` — Fixed inverted color encoding and missing don't-care bits
- `s24ToPixels()` — Fixed handling of `$20` space char and `$80` black-with-SG-flag

---

## v1.0 — Initial Release
**Released:** 2025

### Features
- Dark CRT-green theme
- Basic pencil, fill, rectangle, circle, line, eraser tools
- 8-color + black palette (initially with incorrect index ordering)
- S24 file load/save
- PNG and BMP export
- Image-to-SG24 conversion with Atkinson, Floyd-Steinberg, Bayer, and Blue Noise dithering
- Zoom 1×–16×
- Undo/Redo
