# m3e-canvas Visual Audit & Case Study Canvas Mapping Report

> **Date:** September 20, 2026  
> **Target:** `m3e-canvas` (`http://localhost:3000/`)  
> **Source Repository:** `d:\MAJOR-NODES\zOTHER-PEOPLE\m3e-canvas`  
> **Objective:** Audit `m3e-canvas`'s infinite canvas, parts palette, device framing, inspector editing, and vibe-coding export; and derive how these exact canvas mechanics translate to authoring Growth.Design comic case studies.

---

## 1. Executive Summary

`m3e-canvas` is an open-source web application (Next.js 16 + React 19 + Motion + Tailwind 4) designed for sketch-wireframing Material 3 Expressive mobile and desktop interfaces and turning them into structured AI vibe-coding prompts.

By running and auditing `m3e-canvas` locally on `http://localhost:3000/`, we analyzed how it implements:
1. **Infinite 2D Workspace**: Smooth pan/zoom canvas using CSS transforms, dot-matrix background, and spring physics.
2. **Device Frames as First-Class Entities**: Multiple device screens (`Screen 1`, `Screen 2`) laid out horizontally with standard gaps (`FRAME_GAP = 80px`).
3. **Parts Palette**: Categorized component catalog (Actions, Navigation, Containment) with search, favorites, and click/drag insertion.
4. **Context-Aware Property Inspector**: Two-way binding for editing labels, variants, corner radius, 3x3 layout alignment, and color tokens.
5. **AI Prompt / Spec Generator**: Compiling canvas state into clean, structured prompts for downstream code generation.

---

## 2. Visual Audit Catalog

The following high-fidelity screenshots were captured during live browser execution and are stored in this directory:

### [`01_m3e_canvas_initial.png`](./01_m3e_canvas_initial.png)
**Initial Canvas Workspace & Layout**
- **Far-Left Sidebar**: Tool navigation tabs (Parts, Layers, Colors, Shape, Typography, Motion, AI Prompt, Language).
- **Center Canvas**: Infinite dot grid with viewport navigation controls (Hand tool, Zoom In/Out, Percentage, Fit to Screen).
- **Default Device Screen**: Centered Android mobile frame (`Home`, 412×917) populated with starter components.
- **Right Inspector Drawer**: Clean property editing panel.

---

### [`02_m3e_canvas_palette_and_parts.png`](./02_m3e_canvas_palette_and_parts.png)
**Parts Palette & Categorized UI Library**
- Categorized accordions: **Actions** (Button, Icon Button, FAB, Split Button, Chip), **Navigation** (Top App Bar, Nav Bar, Nav Rail, Toolbar, Tabs, Search Bar), and **Containment** (Cards, Dialogs, Bottom Sheets).
- Live search bar and favoriting (`star`) system.
- Direct click or pointer drag onto the active canvas frame.

---

### [`03_m3e_canvas_inspector_editing.png`](./03_m3e_canvas_inspector_editing.png)
**Component Selection & Real-Time Inspector Editing**
- Selecting a component (e.g. `Favorite` button) brings up property tabs:
  - **Text & Icon**: Live label string editing and Material Symbols icon picker.
  - **Style Variants**: Material 3 visual variants (Filled, Elevated, Tonal, Outlined, Text).
  - **Dimensions & Scale**: Width/height sliders, scale presets (XS to XL), and size modes (Auto, Half, Standard, Full).
  - **Alignment Matrix**: 3×3 visual positioning grid.

---

### [`04_m3e_canvas_device_frame_wireframe.png`](./04_m3e_canvas_device_frame_wireframe.png)
**Multi-Screen Wireframing & Canvas Layout**
- Adding multiple screens (`Home`, `Screen 2`) arranged side-by-side on the infinite canvas.
- Smooth canvas zoom and fit-to-screen (`Fit Screen` button).
- Device framing toggles between mobile phone aspect ratios and desktop viewports.

---

### [`05_m3e_canvas_prompt_export.png`](./05_m3e_canvas_prompt_export.png)
**AI Vibe-Coding Prompt Generation & Export**
- Right-hand **Prompt** drawer automatically compiles the entire canvas layout into structured markdown specs.
- Outputs target platform code guidelines (Android Native Compose or Web React), color tokens (Primary `#6750A4`, Secondary `#635A75`), typography scales, and component hierarchies.
- **"Ask an AI (BETA)"** modal integration for chatting with external LLMs to modify screens.

---

## 3. Deep-Dive: How `m3e-canvas` Implements Its Canvas

### A. Coordinate System & Transform Pipeline (`app/Editor.tsx`)
```ts
// Canvas transformation state:
const [pan, setPan] = useState<{ x: number; y: number }>({ x: 0, y: 0 });
const [zoom, setZoom] = useState<number>(1);

// Applied directly to the canvas viewport container:
style={{
  transform: `translate(${pan.x}px, ${pan.y}px) scale(${zoom})`,
  transformOrigin: "0 0",
}}
```
- Panning is triggered via Middle Mouse Drag, Space + Left Click Drag, or the Hand tool (`H`).
- Zooming is centered around the cursor position using pointer wheel delta.

### B. Frame Architecture (`lib/project.ts`)
```ts
export interface Frame {
  id: string;
  name: string;
  x: number;
  y: number;
  w?: number;
  h?: number;
  place?: Place;
  note?: string;
}
```
- Frames are isolated bounding boxes in canvas space.
- Components are positioned relative to their parent frame, so moving a frame automatically moves all child components inside it.

### C. Drag-and-Follow Pointer Pipeline (`components/CardStage.tsx`)
```ts
function follow(e: React.PointerEvent, onMove: (ev: PointerEvent) => void, onEnd: () => void) {
  e.preventDefault();
  e.stopPropagation();
  const up = () => {
    window.removeEventListener("pointermove", onMove);
    window.removeEventListener("pointerup", up);
    window.removeEventListener("pointercancel", up);
    onEnd();
  };
  window.addEventListener("pointermove", onMove);
  window.addEventListener("pointerup", up);
  window.addEventListener("pointercancel", up);
}
```
- Lightweight, rock-solid dragging without heavy third-party drag-and-drop libraries.
- Reads live component state via refs (`live.current = item`) to avoid stale closure bugs during drag gestures.

---

## 4. Translation Matrix: Wireframing App UI ➔ Designing Case Study Slides

Here is the exact mapping between how `m3e-canvas` operates for UI wireframing and how **Growth Design Studio** operates for comic case study authoring:

| Feature / Dimension | `m3e-canvas` (App UI Wireframing) | `Growth Design Studio` (Case Study Authoring) |
|---|---|---|
| **Primary Goal** | Wireframing interactive app screens & exporting prompts | Authoring psychological teardown comic case studies |
| **Canvas Entity** | Mobile/Desktop Device Frame (`412×917` phone / `1280×800` desktop) | Case Study Slide Canvas (`1280×720` fixed comic viewport) |
| **Canvas Layout** | Freeform 2D plane with multiple screens scattered or side-by-side | Storyboard/Filmstrip sequence of slides (`Slide 1` ➔ `Slide 2` ➔ `Slide 3`) |
| **Component Granularity** | Micro UI controls: Buttons, Chips, App Bars, Cards, FABs | Narrative comic blocks: iPhone/Laptop bezels, App screenshots, Speech bubbles, Dan Bitmoji avatars, Psych meters, Looping tap hands |
| **Temporal Dimension (Time / Animation)** | Static wireframe or basic click-through prototype | **Beat Engine**: Reveal fragments (`data-fragment-index`) stepping through dialogue reveals, avatar emotion swaps, and meter shifts |
| **Parts Palette** | UI Widget catalog categorized by Material 3 intent (Actions, Nav, Inputs) | Slide Archetypes (Cover, Phone, Laptop, Observation, Pro Tip, Outro) + Elements (Bubble, Tap Hand, Psych Meter) |
| **Inspector Panel** | Widget styling: Corner radius, tonal variants, elevation, 3×3 alignment | Storytelling styling: Dan avatar emotions (Neutral, Curious, Thinking, Delighted, Bewildered), dialogue copy, psych meter % level, beat cards |
| **Export / Output** | Vibe-coding LLM Prompt (Markdown / JSON tokens) | Production-ready Reveal presentation (`index.html` + `deck.json`) running with keyboard navigation |

---

## 5. Key Architecture Lessons for Growth Design Studio

From this audit of `m3e-canvas`, we identify 4 concrete engineering patterns that will elevate the Growth Design Studio in subsequent milestones:

### 1. The Multi-Slide "Storyboard" Canvas View (from `m3e-canvas` Multi-Screen)
- **Current Studio**: Center stage shows a single 1280×720 slide iframe at a time, switched via the bottom filmstrip.
- **Adopting `m3e-canvas`**: Implement an optional "Storyboard Mode" where all 5–15 slides of a case study are rendered side-by-side on an infinite 2D canvas (with 80px gap), allowing authors to zoom out and see the entire narrative arc at a glance.

### 2. Lightweight `follow()` Pointer Dragging (from `components/CardStage.tsx`)
- `m3e-canvas` avoids complex drag-and-drop libraries by using a simple, resilient `follow()` helper attached to `pointermove` and `pointerup`.
- We can use this exact pattern to let authors drag speech bubbles, Dan Bitmoji avatars, and the looping tap hand directly on the 1280×720 slide canvas to position them visually.

### 3. Visual Tap Gripper (from `components/CardStage.tsx` Grip Pattern)
- In `m3e-canvas`, the image on a card has a visual handle/grip that lights up and can be dragged to any edge or center.
- In Growth Design Studio, on phone and laptop slides, the **looping tap hand** can have a visual anchor grip on the screen mockup: authors drag the hand directly to whatever button on the app screen Dan is supposed to tap!

### 4. Category-Driven Parts Palette (from `components/PartsPalette.tsx`)
- `m3e-canvas`'s `Tile` and `Section` structure (with icons, labels, and clean hover highlights) aligns directly with our new `Palette.tsx` in `studio/src/components/Palette.tsx`.
- We can expand the Studio palette to include:
  - **Archetypes**: Cover (keys + hand), Phone, Laptop, Observation, Pro Tip, Outro.
  - **Comic FX**: Speech bubble tails (bottom, left, right, large), BAM/OOF bursts, Gold checkmark stamps, Post-it notes.
  - **Dan Emotions**: Direct drag-and-drop of avatar emotions from the palette onto the canvas.
