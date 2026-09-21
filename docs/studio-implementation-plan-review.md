# Review: Studio UX vs m3e-canvas

**Date:** 2026-09-20  
**Studio shots:** `studio/audit/21`–`23` (new) plus older `01`–`20`  
**m3e shots:** `m3e-canvas/audit/01`–`03`  
**Plan Gemini executed:** UX overhaul (contextual inspector, one beat strip, short slide settings, gold canvas)

---

## Verdict

**The four directives landed.** The new shots are a real step toward m3e calm. The right rail is no longer a textbook. TopBar no longer has a second beat stepper.

It is **not** as airy as m3e yet (left palette + filmstrip + beat strip on the board). That is leftover density, not a failed pass.

---

## Checked against the new captures

| Directive | Shot | Result |
|---|---|---|
| Balloon → right rail is only balloon | `22_ux_directive1_balloon_only_right_rail.png` | **Pass.** Header `Speech Balloon`, dialogue, Dan mood, “when does this appear,” remove. No palettes, no stack, no “How This Slide Plays.” |
| One beat UI under the board | `23_ux_directive2_single_beat_strip.png` | **Pass.** `BEATS · 0: Open · 1: Dialogue · 2: Tap` under the 1280 board. TopBar is only deck, save, present. |
| Empty click → short Slide Settings | `21_ux_directive3_slide_settings_short.png` | **Pass.** Cover: title/subtitle (allowed), room color, atmosphere photo, visual stack. No essay. |
| No group badge, gold not purple | all three | **Pass.** No `Unlinked: Avatar + Bubble` on the comic. Amber selection, gold beats. |

Code matches: `Inspector.tsx` early-returns on `selectedElement === 'bubble'`. `TopBar.tsx` dropped beat props in the render. `Canvas.tsx` mounts `.storyboard-beat-strip`. Double-click exists on the balloon in `SlideCanvas.tsx` (not shown in these three stills).

---

## Still not m3e (do not reopen the old plan)

- m3e’s canvas is mostly empty. Studio still has a fat left rail and a filmstrip. Fine for a story tool; just know it will never look like one phone on a white table.
- Beat strip sits **on** the artboard (with zoom). Works; can eat the bottom of a 720 board when zoomed to fit.
- Balloon inspector also has **Dan mood**. Related, not a wall. Optional later: mood only when Dan is selected.
- `CanvasOverlay.tsx` still exists so old Python gates pass. It must **not** be mounted in Edit. `Canvas.tsx` comment says it is not. Keep it that way.
- Those four `validate_*` scripts are overlay-era. They are not proof of this UX. The three PNGs are.

---

## Done for this pass

Yes, for the four directives we asked for. Next work should be **using** the studio (drag, beats, Present), not another chrome rewrite unless something in 21–23 is still wrong in the live app.
