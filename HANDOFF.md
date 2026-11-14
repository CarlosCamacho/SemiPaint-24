# Current Baton Pass

- **Current State:** `semipaint24.html` v4.0 is complete and working — single self-contained HTML/JS/CSS file implementing a full SG24 image editor with pencil, fill, line, rectangle, circle, eyedropper, pan tools; 8-color MC6847 palette with full/left/right sub-modes; image conversion with 7 dithering algorithms; and export to `.S24`, `.BAS` (hex-packed, fits in CoCo RAM), `.asm`, `.png`, and `.bmp`. All known v4 bugs fixed (conversion checkerboard, checker strength slider, grid visibility, BASIC OM error, preview button).
- **Immediate Next Step:**
  - Review `BACKLOG.md` for the highest-priority open bug or planned feature and implement it in `semipaint24.html`
  - Add undo/redo stack (Ctrl+Z / Ctrl+Y) — currently not implemented; this is likely the most impactful missing feature
- **Dead Ends / Do Not Repeat:**
  - Do NOT use pixel-level (64×192) processing for image conversion — it forces left/right half-cells everywhere and produces checkerboard artifacts; always process at cell level (32×192)
  - Do NOT use decimal DATA values in BASIC export — produces 600+ lines and an OM error; always use hex-string packing (192 lines × 64-char hex string)
  - Do NOT store left pixel in bit1 and right pixel in bit0 of their respective slots — the asymmetric model broke left/right-only painting; use the uniform `ep(c,on)=(c&7)<<1|(on?1:0)` encoding for both slots
- **Verification:** Open `semipaint24.html` in a browser and confirm: palette swatches clickable, pencil draws, left/right-only sub-modes render correctly, Convert modal previews, BASIC export downloads as `.BAS`.
