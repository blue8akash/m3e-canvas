# Handoff: Studio canvas vs m3e-canvas

**Date:** 2026-09-20  
**For:** the next agent working on Growth Design Studio  
**This repo:** `d:\MAJOR-NODES\zOTHER-PEOPLE\m3e-canvas` (reference — how a real canvas feels)  
**The app to fix:** `d:\MAJOR-NODES\PRODUCT-RD\Growth-Design-Engine\studio`

Related design (Edit vs Present, JSON, Reveal.js): [case-study-studio.md](./case-study-studio.md)  
Review of the Gemini “real block canvas” plan: [studio-implementation-plan-review.md](./studio-implementation-plan-review.md)

Kit of pieces: `d:\MAJOR-NODES\PRODUCT-RD\Growth-Design-Engine\Growth Design Case\Case Studies\SLIDE_COMPONENTS_CATALOG.md`

---

## What the human wants

A **case-study studio**, not a Material screen editor.

- One 1280×720 slide as the canvas
- Drag catalog parts onto the slide (phone, balloon, tap hand, avatar, …)
- Move those parts on the board the way you move a button in m3e-canvas
- **Present** is Reveal.js, not the editor

They do **not** want one app that does both M3E screens and case studies.  
They do **not** want another architecture essay. They want the studio canvas to actually drag.

---

## What is happening right now

m3e-canvas **works**. Studio **does not feel like a canvas**.

| | m3e-canvas | Studio today |
|---|---|---|
| Run | `http://localhost:3000` (`npm run dev` in this repo) | UI: `http://localhost:5173` (`npm run dev` in `studio/`). API: `http://127.0.0.1:8766` (`python server/server.py`) |
| Board | Real React nodes (the button **is** the button) | A **Reveal iframe** (a picture of the compiled slide) |
| Drag | Pointer down on the part; the part follows | Dashed **overlay boxes** on top of the iframe |
| What you can grab | Any part, any screen | Only overlay hitboxes: bubble, tap hand, avatar, cover keyboard |
| Phone / meter / title / room | Draggable (they are nodes) | Dead. They live in the iframe with `pointer-events: none` |
| After a move | The same node has new x/y | Overlay moves, then save → compile → iframe reload |

On 2026-09-20, m3e-canvas was up on port 3000. Studio Vite on **5173 was down**. Only the Python API on 8766 was up. If the human says “nothing drags,” first check that **both** Studio processes are running.

---

## Why m3e-canvas feels right

Read these, do not clone the whole app:

| Need | File |
|---|---|
| Pointer down on a part | `app/Editor.tsx` → `onItemPointerDown`, `onPartPointerDown` |
| Move from **press origin** | `app/Editor.tsx` ~1918: `dx = (e.clientX - g.sx) / z` |
| Drag a whole phone frame | `onFramePointerDown` + gesture `kind: "frame"` |
| Palette press → ghost follows pointer | `onPartPointerDown` + `PartsPalette.tsx` |
| Tiny `follow()` (window pointer until up) | `components/CardStage.tsx` (forwards the **event**, caller computes origin delta) |

Pattern:

1. The object on screen **is** the document object.
2. Press stores start mouse (`sx`, `sy`) and start position (`gx`, `gy`).
3. Each move: `newX = gx + (clientX - sx) / zoom`.
4. Drop writes that x/y. No second renderer. No compile step to “see” the move.

---

## Why Studio fails (root cause)

Studio authoring is **not a canvas**. It is:

```text
Reveal iframe  (compiled index.html, pointer-events: none)
     +
dashed overlay (a few hitboxes)
```

Key files:

| File | Role |
|---|---|
| `studio/src/components/Canvas.tsx` | Scales a 1280×720 iframe + overlay |
| `studio/src/components/CanvasOverlay.tsx` | Hitboxes for bubble / tap / avatar / keyboard_group |
| `studio/src/lib/drag.ts` | `follow()` |
| `studio/src/components/Board.tsx` | Save JSON → compile → bump `compileVersion` (iframe reload) |
| `studio/server/compiler.py` | JSON → Reveal HTML |

Rules they locked in `studio/PASS_DRAG_OVERLAY.md` (this is the trap):

- Drag **only** bubble, tap, avatar
- Overlay sits **on** the iframe
- Present = real Reveal, **no** overlay
- After drag: write `deck.json`, compile, overlay reads JSON (not iframe DOM)

That pass made a lab, not a studio. Clicking the phone does nothing because the phone is a picture.

---

## Bug that was already fixed (do not redo)

**File:** `studio/src/lib/drag.ts`

`follow()` used to send **incremental** deltas (this event minus last event).  
`CanvasOverlay` added each tiny step to the **original** start left/top.

Result: the dashed box jittered in place. Drag felt broken even on the overlay.

**Fix already applied:** deltas are from the **press origin**, same as m3e:

```ts
dx = (ev.clientX - originX) / scale
dy = (ev.clientY - originY) / scale
```

Callers still do `startLeft + delta.dx`. Palette ghost uses `ev.clientX` / `ev.clientY`, so it still works.

If overlay boxes still do not follow the mouse, this file was reverted. Check origin-based math first.

---

## What still needs to be resolved

This is the real work. Do **not** polish the iframe overlay and call it done.

### 1. Edit must be real blocks on the board (required)

In **Edit**, stop using the Reveal iframe as the thing you drag on.

- Render catalog blocks as DOM on a 1280×720 stage (same idea as m3e parts).
- Pointer down on the **block** moves that block.
- JSON is the document (`Story → Slide → Block` with `x, y, w, h`).
- CSS classes from the catalog (`contained--iphone-14-pro--portrait`, `speech-balloon-container`, `hud__cursor--animated`, …).

Reveal.js runs only in **Play / Present**.

Details: [case-study-studio.md](./case-study-studio.md).

### 2. Palette drop must place a block, not flip a slide type

Today, dropping Phone often only sets `screen_img` / `layout` on the current slide. Cover must stay Cover if you drop a phone onto it.

A drop should **add a block** at the pointer, with catalog defaults.

### 3. Every catalog piece must be grabbable

Not only bubble / tap / avatar. At least:

- Device frame (phone / laptop)
- Product screen inside the frame
- HUD avatar + jumbo character
- Speech balloon + yellow caption
- Tap hand
- Psych meter
- Callout card
- Cover keyboard group (keys PNG + looping hand + caption — three pieces grouped)
- Stickers (✓, ✕, post-it)

See `SLIDE_COMPONENTS_CATALOG.md`.

### 4. Motion stays data on the block

Fragments (click steps), locked ids (auto-animate), loops (tap hand).  
The editor shows a beat scrubber. Reveal **runs** those in Present. Do not fake fragments with a second animation engine.

### 5. Do not clone this whole m3e-canvas repo into Studio

Copy small organs (gesture origin math, palette press, later undo).  
Do not bring Material parts, 412×917 frames, or prompt export.  
Studio already said this in `studio/HOW_WE_USE_M3E_CANVAS.md`. Keep that split.

---

## How to run both (for comparison)

**m3e-canvas (reference):**

```bash
cd d:\MAJOR-NODES\zOTHER-PEOPLE\m3e-canvas
npm run dev
```

Open `http://localhost:3000`. Drag a button. Drag a phone frame. Drag a part from the left rail.

**Studio (the product to fix):**

```bash
cd d:\MAJOR-NODES\PRODUCT-RD\Growth-Design-Engine\studio
python server/server.py
npm run dev
```

Open `http://localhost:5173`. Until (1) is done, you can only grab dashed overlay boxes, and only if Vite is actually running.

---

## Done when

A person can:

1. Open Studio, see one 1280×720 slide.
2. Drag **Phone** from the palette onto the slide; the phone is a real frame they can grab.
3. Drag **Speech balloon** and **Tap hand**; they move under the pointer with no compile wait.
4. Click **Present**; Reveal.js plays the same positions, including beats.

If they still have to hunt for a dashed box on top of an iframe, it is not done.

---

## Status

| Item | Status |
|---|---|
| Why drag felt dead (origin vs incremental `follow()`) | Fixed in `studio/src/lib/drag.ts` |
| Studio Vite must be running on 5173 | Process, not code |
| Iframe + overlay as the editor | **Not resolved** — this is the product gap |
| Real block canvas in Edit, Reveal in Present | **Not built** |
| This m3e-canvas repo still sketches Material screens | Unchanged |
