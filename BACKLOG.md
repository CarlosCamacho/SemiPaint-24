# SemiPaint24 — Backlog

Items are grouped by category. Priority: 🔴 High · 🟡 Medium · 🟢 Low · 💡 Idea

---

## 🐛 Known Bugs

| # | Priority | Description | Notes |
|---|---|---|---|
| B-01 | 🔴 | Flood fill does not respect snap-to-grid boundaries | Should snap fill origin to grid |
| B-02 | 🟡 | Eraser with Left/Right sub-mode leaves the other half of cell untouched | May be correct behavior — needs UX decision |
| B-03 | 🟡 | Very large images converted to SG24 may stall browser for >1 second | Conversion is synchronous; consider Web Worker |
| B-04 | 🟡 | Zoom-to-fit doesn't account for 4:3 aspect ratio stretch | Width calculation should use `WP * sx` |
| B-05 | 🟢 | Mouse cursor position can drift 1px at high zoom levels due to fractional rounding | Low impact |
| B-06 | 🟢 | Grid opacity slider in settings affects cell and pixel grid equally but labels imply independence | Cosmetic |

---

## 🚀 Planned Features

### Drawing
| # | Priority | Feature | Description |
|---|---|---|---|
| F-01 | 🔴 | **Selection tool** | Rectangle select, move, copy, paste region |
| F-02 | 🔴 | **Text stamp tool** | Render VDG charset (from charset.rom) onto canvas as SG24 cells |
| F-03 | 🟡 | **Spray can / airbrush** | Random scatter within brush radius |
| F-04 | 🟡 | **Brush size** | 1–5 cell pencil/eraser brush width |
| F-05 | 🟡 | **Symmetry mode** | Horizontal, vertical, or 4-way mirror drawing |
| F-06 | 🟡 | **Polygon tool** | Click multiple points to define polygon outline/fill |
| F-07 | 🟢 | **Curve tool** | Bezier or arc curve drawing |
| F-08 | 🟢 | **Pattern fill** | Fill with repeating dither patterns instead of solid color |

### Color & Palette
| # | Priority | Feature | Description |
|---|---|---|---|
| C-01 | 🔴 | **CSS=1 color mode** | Toggle between CSS=0 and CSS=1 MC6847 color sets |
| C-02 | 🟡 | **Color replace** | Replace all occurrences of one color with another |
| C-03 | 🟢 | **Recent colors** | Show last 6 used color+sub combinations |

### Image Operations
| # | Priority | Feature | Description |
|---|---|---|---|
| I-01 | 🔴 | **Flip horizontal / vertical** | Mirror the whole canvas |
| I-02 | 🔴 | **Shift / scroll canvas** | Move image content up/down/left/right with wrap |
| I-03 | 🟡 | **Crop / paste from clipboard** | Paste from system clipboard (navigator.clipboard API) |
| I-04 | 🟡 | **Brightness / contrast post-edit** | Adjust rendered image non-destructively after drawing |
| I-05 | 🟡 | **Reduce colors** | Quantize current canvas to fewer than 8 colors |
| I-06 | 🟢 | **Rotate 90°** | Rotate canvas (will distort due to cell aspect ratio) |

### Conversion
| # | Priority | Feature | Description |
|---|---|---|---|
| V-01 | 🔴 | **Conversion comparison view** | Show multiple dithering results side-by-side for comparison |
| V-02 | 🟡 | **Scale modes** — crop, fit, stretch | Currently stretches; add letterbox/crop options |
| V-03 | 🟡 | **Gamma correction input** | Pre-compensate for CoCo NTSC display gamma |
| V-04 | 🟡 | **Palette-restricted conversion** | Convert using only a user-selected subset of the 8 colors |
| V-05 | 🟢 | **Sierra dithering** | Additional error-diffusion variant |
| V-06 | 🟢 | **Jarvis-Judice-Ninke dithering** | Additional error-diffusion variant |

### File & Export
| # | Priority | Feature | Description |
|---|---|---|---|
| E-01 | 🔴 | **Project save/load (.json)** | Save full editor state including undo history |
| E-02 | 🟡 | **Export to CAS tape format** | Wrap BASIC program in CoCo cassette tape binary format |
| E-03 | 🟡 | **Export to DSK disk image** | Embed `.S24` file inside a CoCo disk image for emulators |
| E-04 | 🟡 | **Clipboard copy** | Copy canvas as PNG to system clipboard |
| E-05 | 🟢 | **Animated GIF export** | Export multi-frame animation if layers are implemented |
| E-06 | 🟢 | **XroarJS drag-and-drop** | Generate a URL that auto-loads in the XRoar web emulator |

### UI & UX
| # | Priority | Feature | Description |
|---|---|---|---|
| U-01 | 🔴 | **Undo history panel** | Visual list of undo steps with thumbnails |
| U-02 | 🔴 | **Keyboard shortcut reference overlay** | Press `?` to show all shortcuts |
| U-03 | 🟡 | **Resizable sidebar** | Drag the sidebar edge to make it wider/narrower |
| U-04 | 🟡 | **Floating toolbox mode** | Detach tools into a floating palette like classic paint programs |
| U-05 | 🟡 | **Dark theme option** | Toggle between light (default) and dark/CRT-green themes |
| U-06 | 🟡 | **Ruler / coordinate overlay** | Cell/pixel ruler along canvas edges |
| U-07 | 🟢 | **Tutorial walkthrough** | First-launch guided tour of tools and SG24 concepts |
| U-08 | 🟢 | **Internationalization** | French / Japanese / Spanish UI strings |

### CoCo Integration
| # | Priority | Feature | Description |
|---|---|---|---|
| K-01 | 🟡 | **DSK browser** — load `.S24` from DSK image | Parse CoCo disk image and list `.S24` files |
| K-02 | 🟡 | **VDG text rendering** — charset.rom integration | Stamp MC6847 VDG characters onto canvas using the 3072-byte charset ROM |
| K-03 | 🟡 | **SG8 / SG12 import** | Load and convert SG8 or SG12 mode screen files |
| K-04 | 🟢 | **BASIC tokenizer preview** | Show approximate tokenized byte count of `.BAS` export |
| K-05 | 🟢 | **CoCo 3 GIME mode support** | Extend to support CoCo 3 high-resolution modes (future separate editor) |

---

## 💡 Ideas / Research

- **AI-assisted color palette** — Run a k-means pass on the source image limited to the 8 SG24 colors before dithering for better color mapping
- **SG24 animation** — Rapidly POKE alternating screens for simple animation effects; add timeline/frame editor
- **WebSocket CoCo link** — Live stream screen data to a CoCo via a serial/USB bridge
- **NTSC composite emulation** — Display the canvas with simulated NTSC color bleed and scanlines for authentic preview
- **Sprite sheet to SG24** — Import a sprite sheet and break it into individual SG24-sized tiles
- **Community palette presets** — Curated color usage presets optimized for portraits, landscapes, etc.

---

## ✅ Completed (moved from backlog)

| Version | Item | Description |
|---|---|---|
| v4.0 | ~~Cell-level conversion~~ | Rewrote conversion to process 32×192 cell grid instead of 64×192 pixel grid |
| v4.0 | ~~Checker strength~~ | Was non-functional; now correctly darkens alternating cells |
| v4.0 | ~~BASIC OM error~~ | Replaced decimal DATA with hex-string packing; 3× more compact |
| v4.0 | ~~Grid visibility~~ | Fixed sub-pixel lineWidth; grid now visible at all zoom levels |
| v4.0 | ~~Accordion sidebar~~ | All sidebar panels are collapsible accordions |
| v4.0 | ~~2-column palette~~ | Palette redesigned to 2-column layout with larger swatches |
| v4.0 | ~~Aspect ratio toggle~~ | CoCo 4:3 vs square pixels in View menu |
| v3.0 | ~~Color index bug~~ | Fixed Green=0 (was Black=0) per MC6847 datasheet |
| v3.0 | ~~Left/right pixel bug~~ | Rewrote internal encoding to uniform bit1 model |
| v2.0 | ~~S24 byte format~~ | Confirmed from real `.S24` file analysis |
| v2.0 | ~~Undo/Redo~~ | 50-level undo stack |
