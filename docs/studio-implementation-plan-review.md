# Review: Studio plans

**Date:** 2026-09-20 (fourth plan — “de-chaotify”)  
**Plan file:** `c:\Users\BlueSpace\.gemini\antigravity-ide\brain\1416cd13-2713-44f5-96d7-67b4ccc98f1a\implementation_plan.md`  
**Product:** `d:\MAJOR-NODES\PRODUCT-RD\Growth-Design-Engine\studio`  
**Background:** [studio-canvas-handoff.md](./studio-canvas-handoff.md) · [case-study-studio.md](./case-study-studio.md)

---

## Verdict

**Yes — this is the right pass for “Studio feels chaotic.” Execute it, with one extra requirement.**

The chaos is not the iframe anymore. Edit is `SlideCanvas`. The chaos is **too much UI at once**, and **beats that do not mean anything on the board**.

Do inspector-by-selection, calm palette, named beats, hover.  
**Also hide/show artboard pieces by beat.** A TopBar that says “Speech bubble appears” while the bubble is always visible is still chaotic.

Do **not** bring back zoom-world, overlay, or Material purple.

---

## What is true in code today

| Piece | State |
|---|---|
| Edit | `SlideCanvas.tsx` — grab the node |
| Iframe in Edit | Gone from `Canvas.tsx` |
| `CanvasOverlay.tsx` | Dead file. Do not mount it |
| Inspector | Takes `selectedElement`, shows a **badge**, still dumps **all** slide fields (dialogue, screen, avatars, meter, beats, cover, outro…) |
| Palette | Flat grid, subtitles like `§3 contained--iphone--portrait` |
| TopBar beats | `BEAT 2 / 3` only. Prev/next still `postMessage` to a missing `iframeRef` |
| `inspectAllActive` | TopBar only. Never reaches `SlideCanvas` |
| SlideCanvas vs beats | Uses beat for tap x/y and dialogue **text**. Does not hide balloon/hand until that beat |
| Selection | Purple box, fake corner dots |
| `follow()` | Origin-based. Keep it |

That matches the plan’s five chaos causes. The plan missed the sixth: **the artboard does not step.**

---

## What to build (from the plan)

### 1. Context inspector — required

`Inspector.tsx` already knows `selectedElement`. Stop showing everything.

| Selection | Panel |
|---|---|
| `bubble` | Dialogue, X/Y, W/H, tail |
| `avatar` | Emotion picker, X/Y |
| `phone` / `laptop` | Screen image, X/Y (phone: center/left). **No W/H stretch on iPhone bezel** |
| `meter` | On/off, level, beat delta, X/Y |
| `tap` | `{tap}` / `{click}` text, X/Y |
| `caption` | Text, X/Y, W/H |
| `keyboard_group` | X/Y |
| `null` | Slide overview + beat timeline only. Cover title, pro tip, outro live **here**, not on every selection |

Add **Deselect**. Numeric X/Y must write the same fields drag writes (`bubble_left`, …).

Keep gold (`#14100b`, `#fbbf24`). Do not restyle as m3e purple.

### 2. Palette categories — required, cheap

Four groups, human subtitles (`iPhone 334×720 bezel`, not `§3 contained--…`). Same drop behavior. Do not change catalog kinds.

### 3. TopBar beat label — required, not enough alone

Show `BEAT 2 OF 3 • Speech bubble appears` from `beats[i].desc`.

Also:

- Pass `inspectAllActive` into `SlideCanvas`
- Show pieces for this beat or earlier (unless Inspect All)
- Swap `activeBeat.avatar` and meter variation
- **Remove** `iframeRef` / `postToIframe` from `Board.tsx`

### 4. Hover + real handles — yes, with limits

Hover outline on unselected blocks. `grab` / `grabbing`.

Resize **balloon and caption** only. Live W×H badge.  
Do not resize the iPhone bezel. Do not paste m3e `SizeHandles.tsx` whole.

### 5. Filmstrip badges — optional, fine

`#1 COVER` etc., amber active ring, hover delete. Do not block 1–4.

---

## What not to do

- Infinite camera / Space-pan world (previous plan). One 1280×720 slide.
- Mount `CanvasOverlay` or an Edit iframe.
- Copy m3e purple chrome.
- Treat `validate_groups_pass.py` as proof this pass works. `npm run build` + the browser list below.

---

## Done when

On `http://localhost:5173/?deck=mini-my-test-case`:

1. Click balloon → right panel is **only** balloon. Click empty board → slide overview + beats. No wall of cover/outro fields while a phone is selected.
2. Palette shows four groups with plain language, not `§9 hud__cursor--animated`.
3. TopBar: `BEAT 2 OF 3 • Speech bubble appears`.
4. On a phone slide, beat 0 does not show the tap hand if that hand belongs to a later beat. Inspect All shows all. Next-beat does **not** talk to an iframe.
5. Hover a block: outline. Drag still follows. Balloon side handle changes width.

If 1 and 4 fail, the studio is still chaotic.
