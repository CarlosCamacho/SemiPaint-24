# Current Baton Pass

- **Current State:** `index.html` (formerly `semipaint24.html`) v4.0 is complete and working — single self-contained HTML/JS/CSS SG24 image editor with full drawing tools, 8-color MC6847 palette with full/left/right sub-modes, 7 dithering algorithms, and export to .S24/.BAS/.asm/.png/.bmp. All known v4 bugs fixed. 30 GitHub Issues now track all remaining work.

- **Immediate Next Step:**
  - **#1** `[Bug]` Flood fill snap-to-grid (🔴 High, quick fix — snap fill origin to `Math.floor(x/2)*2`)
  - **#11** `[Feature]` Project save/load (.sp24 JSON) — prevents work loss between sessions

- **Dead Ends / Do Not Repeat:**
  - Do NOT use pixel-level (64×192) processing for image conversion — always process at cell level (32×192); pixel-level forces left/right half-cells everywhere and produces checkerboard artifacts
  - Do NOT use decimal DATA values in BASIC export — produces 600+ lines causing CoCo OM error; always use hex-string packing (192 lines × 64-char hex string)
  - Do NOT use asymmetric bit encoding for left/right pixels — broke left/right-only painting; use uniform `ep(c,on)=(c&7)<<1|(on?1:0)` for both slots
  - Do NOT rename `index.html` back to `semipaint24.html` — renamed for GitHub Pages compatibility

- **Verification:** `open index.html` in browser — confirm palette swatches clickable, pencil draws, left/right-only sub-modes render correctly, Convert modal previews, BASIC export downloads as `.BAS`.
