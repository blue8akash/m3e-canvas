# Review: Studio plans

**Date:** 2026-09-20  
**Plan:** `c:\Users\BlueSpace\.gemini\antigravity-ide\brain\1416cd13-2713-44f5-96d7-67b4ccc98f1a\implementation_plan.md`  
**Product:** `d:\MAJOR-NODES\PRODUCT-RD\Growth-Design-Engine\studio`  
**Decks:** `Growth Design Case/Case Studies` · catalog + Labor Perception Bias HTML  
**Background:** [studio-canvas-handoff.md](./studio-canvas-handoff.md) · [case-study-studio.md](./case-study-studio.md)

---

## Verdict

**Yes. Execute this plan.** It matches the decks: lock + auto-animate, fragments stay on beats, keyboard group stays one unit, room is not a block.

Fix the small nits below while building. Do not add `appearBeat` or `inheritFrom` back.

---

## What is right

- No second story clock. Stack = z-index, editor hide, lock. Beats = when pieces appear.
- Phone carry = `phone_locked` + duplicate slide / copy to next. Compiler emits `data-locked="true"`. Same as locked HubSpot phone at ~474×0.
- Stickers duplicate; kit pieces stay one-per-slide. Called out in the plan.
- Dan and balloon stay separate. Cover keyboard stays one group.
- Room stays on the section.
- Default z stack (device 10 → stickers 12 → balloon 14 → tap 16 → HUD 18 → keys 20) matches typical `.sl-block-content` order.
- Eye tooltip says editor-only. Good.
- `npm run build` + live checks, not old overlay Python gates.

---

## Nits (handle in the same pass)

1. **Two lock flags.** `slide.phone_locked` and `stack[].locked` for the same phone. Pick one write path: Inspector “Lock Position” sets **both**, or only `phone_locked` and stack reads it. Otherwise drag lock and compiler drift.

2. **Copy device onto Cover.** “Copy Device to Next Slide” must **skip** cover/outro (or warn). A locked phone on a cover is not a Growth.Design cover.

3. **Ctrl+A list.** Include `laptop` and `keyboard_group`. Plan only lists phone, bubble, avatar, tap, meter, caption, stickers.

4. **`groupId` lives on the sticker.** Do not also require it on `SlideStackItem` unless the stack row is just a mirror. One source of truth: `SlideSticker.groupId`.

5. **First reorder writes `stack[]`.** Until the user changes z-order, derive from defaults. On first ▲/▼, persist a full `stack` for every piece on the slide so later compiles stay stable.

6. **Multi-select.** `SlideCanvas` already has optional `selectedElements`; `Board` still has a single `selectedElement`. Ctrl+C/V/D need Board to keep an **array**. Wire that when adding clipboard.

7. **Compiler.** V1 `data-locked` on phone/laptop is enough. Meter lock in HTML can wait. Put `z-index` on `.sl-block-content` (as the decks do), not only the outer wrapper.

---

## Do not do

- `layers.appearBeat` or `inheritFrom`
- Glue Dan + balloon
- Stretch iPhone bezel
- Mount `CanvasOverlay` or an Edit iframe
- Undo (`Ctrl+Z`) in this pass

---

## Done when

The plan’s own checks pass:

1. Ctrl+D on a post-it → new id at +20,+20; Present shows both.
2. Lock phone → no drag; duplicate slide keeps coords; compiled HTML has `data-locked="true"` and `data-auto-animate`.
3. Stack ▲/▼ changes paint order; beats still show/hide balloon vs tap.
4. Cover keyboard still moves as one. Dan and balloon are not glued.
