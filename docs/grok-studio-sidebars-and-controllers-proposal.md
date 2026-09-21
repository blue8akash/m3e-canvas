# Studio UI Overhaul: Modernizing Sidebars & Component Controllers
### Adapting M3E Canvas Ergonomics to Growth Design Comic Case Studies

**Date:** 2026-09-21  
**Target:** `d:\MAJOR-NODES\PRODUCT-RD\Growth-Design-Engine\studio`  
**Reference Repo:** `d:\MAJOR-NODES\zOTHER-PEOPLE\m3e-canvas` (MIT, lnkiai)  
**Status:** Proposal for Grok Review & Sign-Off  
**Related Documents:**
- `studio/M3E_TO_STUDIO_TRANSFORMATION.md` (Organ Transplant Policy)
- `studio/HOW_WE_USE_M3E_CANVAS.md` (Core Rules)
- `docs/final-component-pass-signoff.md` (Zero-Gap Component Sign-Off)
- `docs/studio-implementation-plan-review.md` (Calm UX Review)

---

## 1. Executive Summary & Intent

Our previous pass completed **100% component drag-and-drop coverage** for all 15 catalog pieces (phones, laptops, balloons, avatars, psych meter, callouts, post-its, badges) and patched native dropdown readability.

However, the user experience of Studio remains visually disjointed and ergonomically frustrating:
1. **The Left Sidebar is "Ugly"**: It is a rigid, bulky 220px slab with wordy technical subtitles, zero search capability, no collapsible drawer, and no icon rail.
2. **The Component Controllers are "Very Confusing"**: `Inspector.tsx` is a 1,784-line monolithic file where visual design (colors, models, text) and story sequence (reveal beats, meter reactions) are haphazardly mixed. Screen presets are five plain-text buttons stacked vertically. Dan’s emotions are 19 giant square buttons that push essential controls off-screen.

**The Goal:** Borrow the sleek, tactile, and calm editor ergonomics of `m3e-canvas` (52px icon rail, 78px visual tiles, instant search, two-tab contextual inspector, sliding capsule pill segmented switches, visual thumbnail strips, compact emotion popovers) while **strictly preserving** Growth Design's authentic comic case-study domain (1280×720 canvas, `deck.json` schema, Python compiler, Reveal.js playback, Dan/Louis characters).

---

## 2. The "Organ Transplant" Architecture

We follow Grok's foundational rule: **"Copy organs, do not clone the body."**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       THE GROWTH DESIGN BODY (KEPT)                         │
│  • 1280×720 Slide Canvas (16:9 Comic Artboard)                              │
│  • deck.json + Python Compiler (compiler.py) -> Reveal.js Playback          │
│  • Authentic Catalog: Speech Balloons, Psych Meter, Dan Avatars, Callouts   │
│  • Story Beats (Linear narrative clicks on a slide, not screen branching)   │
└─────────────────────────────────────────────────────────────────────────────┘
                               ▲ Transplants
┌──────────────────────────────┴──────────────────────────────────────────────┐
│                    M3E-CANVAS ERGONOMIC ORGANS (BORROWED)                   │
│  1. 52px Slim Icon Rail + 280px Collapsible Drawer with Instant Search      │
│  2. 78px Visual Grid Tiles (Icons + clean labels, no paragraph subtitles)   │
│  3. Two-Tab Contextual Inspector: [ 🎨 Design ] vs [ ⚡ Story Beats ]       │
│  4. Segmented<T> Capsule Pill Switches with fused rounded borders           │
│  5. Visual ImagePresetStrip (Horizontal thumbnail cards instead of buttons) │
│  6. Compact EmotionPickerPill (Face badge + popover instead of 19 tiles)    │
│  7. PanelShell with fixed header, action icons, and 28px top gradient fade  │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Current Shortcomings vs. M3E Reference Audit

| Feature Area | Current Studio UI (Flawed) | M3E Canvas Reference (Sleek) | Proposed Studio Overhaul |
|---|---|---|---|
| **Left Rail Width** | Permanently fixed at 220px, wasting canvas space | 52px slim icon rail + 280px collapsible drawer | **52px Rail** (`Parts`, `Layers`, `Slide`, `Decks`) + **280px Collapsible Drawer** |
| **Parts Search** | None. User must scroll through 6 categories | Real-time live fuzzy search input in header | **Instant search input** filtering all 15 parts with hotkey `/` |
| **Part Tiles** | Chunky rectangular cards with long paragraph subtitles | Compact 78px square tiles, bold icon, clean label | **78px square tiles** with high-contrast icon, label, and drag handle |
| **Inspector Structure** | 1,784 lines monolithic `Inspector.tsx`, single long vertical scroll | `PanelShell` with fixed header, top gradient fade, two tabs | Modular sub-inspectors wrapped in **`PanelShell`** with 28px top fade |
| **Inspector Tabs** | Single flat page. Story beat timing dumped at bottom | Two tabs: `Design` (visuals) vs `Behavior/Trigger` | Two tabs: **🎨 Design** (visuals/copy) vs **⚡ Story Beats** (reveal step & meter) |
| **Screen Selection** | 5 vertically stacked text buttons taking 160px | Horizontal visual image strip / card picker | **Horizontal thumbnail strip** (`ImagePresetStrip`) with live image preview |
| **Emotion Selection** | 19 giant square buttons taking 400px+ vertical space | Compact popover / icon picker | **Compact Emotion Pill** (shows active Dan face; click opens 4×4 popover grid) |
| **Model / Style Switch** | HTML `<select>` dropdowns | `Segmented<T>` rounded capsule pills | **`Segmented<T>` pill buttons** for 2–4 choices; custom popover for long lists |
| **Psych Meter Slider** | Raw browser `<input type="range">` | `Slider` with tactile track and floating percentage badge | **Tactile Slider** with color-coded live badge (Green/Amber/Red) |
| **Slide Layers / Stack** | Stuffed into slide settings when nothing is selected | Dedicated `LayersPanel` in left rail | **📑 Layers Drawer** in left rail for reordering, locking, and visibility |

---

## 4. Detailed Component Design & Specifications

### 4.1. UI Primitives Library (`studio/src/components/ui/Controls.tsx`)

To avoid scattered, inconsistent ad-hoc HTML controls across Studio, we extract a unified design system inspired by `m3e-canvas/components/ui.tsx`:

#### A. `Segmented<T>` (Capsule Pill Switcher)
- **Role:** Replaces clunky radio buttons and 2–3 option dropdowns (e.g. Device: `Phone` vs `Laptop`; Balloon: `Speech` vs `Thought` vs `Radio`; Character: `Dan` vs `Louis`).
- **Ergonomics:**
  - Fused pill container (`height: 36px`, `borderRadius: 18px`, `background: var(--bg-surface-2)`).
  - Sliding active pill indicator with smooth spring/cubic-bezier motion.
  - Keyboard accessible (Arrow keys walk options).
  - High-contrast text (`color: var(--accent-gold)` or `#ffffff` on active).

#### B. `ImagePresetStrip` (Horizontal Thumbnail Carousel)
- **Role:** Replaces the 5 ugly vertical text buttons for app mockup screens and background textures.
- **Ergonomics:**
  - Horizontal scrolling row of 54×54 rounded thumbnail cards.
  - Image preview with border outline (`2px solid var(--accent-gold)` when active).
  - Hover tooltip displaying screen title and dimensions.
  - Direct "Upload Custom" tile at the front with a `+` icon.

#### C. `EmotionPickerPill` (Compact Popover)
- **Role:** Replaces the 19 giant emotion buttons that currently destroy the Dan Avatar Inspector.
- **Ergonomics:**
  - Resting State: A single 40px compact pill showing the active Dan avatar face thumbnail, emotion label (e.g. `Delighted`), and a chevron icon.
  - Clicked State: Opens a floating 4×4 popover grid (`width: 240px`, frosted backdrop) with small circular faces categorized by sentiment (Positive, Neutral, Skeptical, Anxious).
  - Selection instantly updates the canvas and closes or stays open for quick preview.

#### D. `TactileSlider` (Responsive Slider with Live Badge)
- **Role:** For Psych Meter (0–100%), Screen Y-Offset, and Zoom.
- **Ergonomics:**
  - Custom track styling (dark graphite track with glowing amber/green fill).
  - Integrated badge showing value (`+15%`, `80%`) that dynamically tints green for positive, amber for neutral, red for negative.

---

### 4.2. Modernized Left Navigation Rail & Drawer (`Palette.tsx`)

```
┌───────┬──────────────────────────────────┐
│  RAIL │ DRAWER (Collapsible, 280px)      │
│ (52px)│                                  │
│       │ ┌──────────────────────────────┐ │
│  [GD] │ │ 🔍 Search components... (/)  │ │
│       │ └──────────────────────────────┘ │
│  [🧩] │ CATEGORY: COMIC STORY            │
│ Parts │ ┌──────────┐ ┌──────────┐        │
│       │ │   💬     │ │   🏷️     │        │
│  [📑] │ │ Balloon  │ │ Caption  │        │
│ Layers│ └──────────┘ └──────────┘        │
│       │ CATEGORY: ACTORS & METERS        │
│  [🖼️] │ ┌──────────┐ ┌──────────┐        │
│ Slide │ │   👤     │ │   🧠     │        │
│       │ │  Avatar  │ │  Meter   │        │
│  [⚡] │ └──────────┘ └──────────┘        │
│ Beats │ ┌──────────┐ ┌──────────┐        │
│       │ │   🧍     │ │   👆     │        │
│       │ │  Jumbo   │ │ Tap Hand │        │
│       │ └──────────┘ └──────────┘        │
│       │ CATEGORY: TEACHING & CARDS       │
│       │ ┌──────────┐ ┌──────────┐        │
│  [⚙️] │ │   💡     │ │   📝     │        │
│ Meta  │ │ Callout  │ │ Sticky   │        │
│       │ └──────────┘ └──────────┘        │
└───────┴──────────────────────────────────┘
```

1. **52px Slim Icon Rail:**
   - Always visible, dark graphite styling (`#14100b` / `rgba(20, 16, 11, 0.95)`).
   - Icons:
     - 🧩 **Parts**: Opens Catalog Drawer (15 draggable pieces).
     - 📑 **Layers**: Opens Slide Elements & z-index reorder tree.
     - 🖼️ **Slide**: Opens Slide Setup (Archetype, Background, Atmosphere).
     - ⚡ **Beats**: Slide narrative timeline & step sequencer overview.
     - 📚 **Decks**: Case study switcher and export settings.
   - Clicking an active rail icon collapses the drawer, allowing full-bleed canvas editing.

2. **280px Collapsible Drawer:**
   - Header with Drawer title and close button (`✕`).
   - Global instant fuzzy search input (`Field`).
   - Categorized sections with 78px square tiles.
   - Pointer-down on any tile triggers the identical `follow()` drag mechanism onto the 1280×720 canvas.

---

### 4.3. Modernized Right Inspector (`Inspector.tsx`)

Instead of one 1,784-line file containing endless conditional code, we structure the Inspector with `PanelShell` and **Two Dedicated Tabs**:

```
┌──────────────────────────────────────────────────────┐
│  PANEL HEADER                                        │
│  💬 Speech Balloon                     [Esc to Slide]│
├──────────────────────────────────────────────────────┤
│  [ 🎨 Design ]            [ ⚡ Story Beats ]         │
├──────────────────────────────────────────────────────┤
│  (Top 28px smooth gradient fade)                     │
│                                                      │
│  TAB 1: DESIGN                                       │
│  • Balloon Style: [ Speech | Thought | Radio ]       │
│  • Tail Direction: [ Bottom-Left | Bottom-Right | Top│
│  • Dialogue Text (Multi-line growth comic input)     │
│  • Associated Avatar: [ Guide (Neutral) ▾ ]          │
│                                                      │
│  ─────────────────────────────────────────────────── │
│  TAB 2: STORY BEATS                                  │
│  • When does this appear on the slide?               │
│    [ Slide Start (0) | Click 1 | Click 2 | Click 3 ] │
│  • Psych Meter Impact Delta:                         │
│    [ -20% | -10% | 0 | +10% | +20% ]                 │
│  • Target Gesture Cue: [ Bind to Tap Hand ]          │
│                                                      │
│  ─────────────────────────────────────────────────── │
│  [ 🗑️ Remove Element ]                               │
└──────────────────────────────────────────────────────┘
```

#### The Two-Tab Split Principle:
- **Tab 1: 🎨 Design (Visuals, Styling & Copy)**
  - What does this element look like?
  - Device: Frame model (iPhone 16 / 14 / X, MacBook), screen screenshot, image upload, screen Y scroll offset.
  - Balloon: Style (Speech, Thought, Radio), tail orientation, dialogue copy text.
  - Callout Card: Variant (`#UX Pro Tip`, `#Psychology`, `#UI`, `#Data`), title, insight body, source citation.
  - Avatar: Character (Dan / Louis), facial expression, pose.
  - Meter: Level slider (0–100%), visibility toggle.
  - Sticky Note: Color tint (Yellow, Green, Red, Blue), handwritten note copy.
- **Tab 2: ⚡ Story Beats (Timeline & Interactive Triggers)**
  - *When* does this element trigger in the story?
  - `reveal_beat`: Step selector (Slide Start / Click 1, 2, 3...) with descriptive label from the slide timeline.
  - Meter Reaction: Does this beat cause delight (+15%) or friction (-20%)?
  - Tap Binding: Associate tap hand cursor directly with this element.

---

## 5. Architectural File Structure

```text
studio/src/
├── components/
│   ├── ui/
│   │   ├── Controls.tsx        <-- NEW: Segmented, ImagePresetStrip, EmotionPickerPill, TactileSlider
│   │   └── PanelShell.tsx      <-- NEW: Fixed header, top gradient fade, tab bar
│   ├── inspector/
│   │   ├── DeviceInspector.tsx   <-- MODULAR: Phone & Laptop controls
│   │   ├── BalloonInspector.tsx  <-- MODULAR: Speech, Thought, Radio balloons
│   │   ├── AvatarInspector.tsx   <-- MODULAR: Dan/Louis HUD & Jumbo actors
│   │   ├── CalloutInspector.tsx  <-- MODULAR: Teaching cards
│   │   ├── MeterInspector.tsx    <-- MODULAR: Psych meter gauge
│   │   ├── SlideInspector.tsx    <-- MODULAR: Archetype & room backgrounds
│   │   └── LayersInspector.tsx   <-- MODULAR: Z-order stack manager
│   ├── Palette.tsx             <-- REFACTORED: 52px Icon Rail + 280px Search Drawer
│   ├── Inspector.tsx           <-- REFACTORED: Clean router mapping to modular inspectors
│   ├── Board.tsx
│   └── SlideCanvas.tsx
└── styles/
    ├── controls.css            <-- NEW: Pill segment, thumbnail strip, popover styles
    └── board.css               <-- CLEANED: High-contrast tokens, dark theme perfection
```

---

## 6. Questions & Decision Points for Grok

We request Grok's critique and guidance on these specific architectural decisions before we execute:

### 1. Two-Tab Inspector Model (`🎨 Design` vs `⚡ Story Beats`)
- **Question:** Does splitting every component's inspector into **Design** (visuals/copy) and **Story Beats** (reveal beat index, meter reaction, trigger hand) provide the optimal balance of power and calm?
- **Alternative:** Single flat scroll with collapsible accordions. (We strongly favor the two-tab model because story timing was what made the inspector feel confusing and endless).

### 2. Emotion Picker Interaction Model
- **Question:** Should the Dan Avatar emotion selector be:
  - **Option A (Recommended):** A compact pill showing the current face thumbnail, which opens a floating 4×4 popover grid when clicked.
  - **Option B:** A fixed 2-row horizontal scrolling carousel directly embedded in the inspector tab.

### 3. Left Rail Drawer Behavior
- **Question:** Should the 280px drawer:
  - **Option A:** Auto-collapse when a part is dropped onto the canvas (maximizing artboard editing room).
  - **Option B (Recommended):** Stay open until explicitly toggled by the user or by clicking outside/rail icon (better for rapid assembly of multiple components).

### 4. Layers / Slide Stack Location
- **Question:** Currently, the visual z-order stack of slide elements is shown in the right inspector when nothing is selected. Should we move this into a dedicated **📑 Layers Drawer** on the Left Rail (like Photoshop/Figma/M3E), keeping the Right Inspector strictly for element properties and slide presentation settings?

### 5. Sign-off on Transition Plan
- **Verification:** Confirm that this overhaul maintains full backwards compatibility with existing `deck.json` structures, Python compiler (`compiler.py`), and the 5 test suites.

---

## 7. Next Steps Upon Sign-Off

1. **Step 1:** Implement `studio/src/components/ui/Controls.tsx` and `controls.css` with zero dependencies beyond existing React/CSS tokens.
2. **Step 2:** Refactor `studio/src/components/Palette.tsx` into the 52px Icon Rail + 280px Searchable Drawer.
3. **Step 3:** Refactor `studio/src/components/Inspector.tsx` into `PanelShell` + modular inspectors with the `Design` / `Story Beats` tab structure.
4. **Step 4:** Run automated validation (`validate_all_components_drag.py`, `npm run build`) and capture Playwright browser audit screenshots (`audit/25_modern_sidebars_audit.png`).
