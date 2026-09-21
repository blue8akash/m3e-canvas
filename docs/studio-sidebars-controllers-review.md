# Critique: sidebars & controllers proposal

**Date:** 2026-09-21  
**Proposal:** `studio/GROK_SIDEBARS_AND_CONTROLLERS_PROPOSAL.md`  
**Related:** [studio-implementation-plan-review.md](./studio-implementation-plan-review.md) · [final-component-pass-signoff.md](./final-component-pass-signoff.md)

---

## Overall

The pain is real: fat left rail, 19 emotion tiles, vertical screen buttons, 1,784-line inspector. Borrowing **pills, search, compact tiles, emotion popover** from m3e is the right organ transplant.

The proposal also **reopens calm-UX debt**. `audit/21`–`23` already won: balloon-only inspector, **one beat strip under the board**, short Slide Settings. This plan adds a left **Beats** drawer, an inspector **Story Beats** tab, and keeps the canvas strip → **three clocks** again.

Copy the controls. Do not copy m3e’s whole chrome (icon rail with five apps). Studio is one 1280×720 comic, not a map of phones.

Gold / graphite. No purple.

---

## Section 6 — decisions

### 1. Two-tab inspector (`Design` vs `Story Beats`)

**Partial yes. Narrow Tab 2.**

Splitting “how it looks” from “when it appears” is right. A **flat accordion wall** is worse. Do **not** put the **slide sequencer** in the inspector.

| On Design (default) | On Story (thin) | Not on this piece at all |
|---|---|---|
| Copy, style, frame, face, color | **Only** `reveal_beat` / `bubble_beat`: Slide start / Click 1 / 2… | Slide beat list (that is the **strip under the board**) |
| | | Meter ±% (that is the **meter** or the **beat**, catalog `meter_variation`) |
| | | “Bind tap hand” (tap is its **own** block) |

If Tab 2 is empty for a piece with no fragment, hide the tab. Phone at slide start should not force a Story tab of dead controls.

**Do not** add a left-rail ⚡ Beats icon. One sequencer: the strip already under the artboard (`audit/23`). Inspector only answers “this balloon appears on which click?”

### 2. Emotion picker

**Option A (pill + popover).** Not B.

19 giant squares are the bug. A 2-row carousel is the same bug sideways.

A 4×4 of 16 faces is too small for **225** harvested avatars. Spec:

- Resting: 40px face + label (`Delighted`)
- Open: popover, **search**, sentiment groups, **scroll** (not a hard 4×4)
- Stay open while clicking faces so the canvas can preview; click outside or Esc to close
- HUD circle vs jumbo pose: same picker, filtered file set

### 3. Left drawer after drop

**Option B: stay open.** Not A.

Assembling a slide is phone → balloon → tap → callout. Auto-collapse after every drop is hostile. m3e’s parts drawer stays open.

Collapse only when:

- User clicks the **same** rail icon again, or
- Hotkey (e.g. `\`) to full-bleed the board

Do **not** collapse on “click outside” — that fights selecting the comic.

Default: Parts drawer **open** (authors live there). 52px rail-only is an opt-in, not the first paint.

### 4. Layers on the left rail

**Yes, move the stack to a Layers drawer. Do not add Slide / Beats / Decks as equal rail apps.**

Empty-click Slide Settings should stay **short and on the right**: room color, atmosphere, title on Cover. That is presentation, not z-order. Layers (z, lock, eye) are a **list** and belong in a left drawer like Figma.

Left rail icons, **two** (three max):

| Icon | Drawer |
|---|---|
| Parts | Catalog + search |
| Layers | Stack for **this slide** |
| (optional) Decks | Only if you **remove** the TopBar deck dropdown. Do not duplicate. |

**Out of the rail:** Slide setup (right, empty click), Beats (canvas strip), Meta/settings (TopBar).

File split `LayersInspector.tsx` can still render **inside the left drawer**, not the right `Inspector.tsx`.

### 5. Transition / compatibility

**Sign off the organ transplant if these constraints hold. Otherwise no.**

Safe (do this):

- `Segmented`, `ImagePresetStrip`, `EmotionPickerPill`, `TactileSlider`, `PanelShell`
- Palette → 52px + 280px search, **gold** tiles, no paragraph subtitles
- Split `Inspector.tsx` into modular files; **no `deck.json` field changes** required
- `follow()` drag unchanged
- Compiler / Reveal unchanged
- `npm run build` + Playwright `audit/25_*.png` proving: balloon-only right rail, **one** beat strip, short slide settings, gold not purple

Unsafe (do not):

- New schema: tap-binding, per-sticker meter delta, inheritFrom, appearBeat on layers
- Treating overlay-era `validate_groups_pass.py` / `validate_drag_pass.py` as UX proof
- Five rail destinations (Parts, Layers, Slide, Beats, Meta)
- m3e purple, Motion-heavy `SizeHandles` paste, infinite camera
- Removing the under-board beat strip “because the inspector has a Story tab now”

`deck.json` stays: stickers, `reveal_beat`, `phone_model`, stack, balloon style fields already signed off. UI only reads them.

---

## Execute order (if they follow this critique)

1. `Controls.tsx` + gold `controls.css` (no new deps).
2. Palette rail: **Parts + Layers** only; search; 78px tiles; drawer stays open on drop.
3. Inspector router + Design tab; Story tab = reveal step **only**; emotion pill; screen thumbnail strip.
4. Screenshot audit vs `21`–`23` plus `25`. Beat strip still the only sequencer.

---

## Short answers for Section 6

1. **Two tabs:** yes, if Story = this piece’s reveal click only. No meter/tap/sequencer in that tab. No left Beats icon.  
2. **Emotion:** A, with search + scroll, not a locked 4×4.  
3. **Drawer:** B, stay open. Collapse is explicit.  
4. **Layers:** left drawer. Slide room stays right. Rail is Parts + Layers, not five apps.  
5. **Compat:** yes if schema-neutral and `audit/21`–`23` still pass. Overlay Python gates are not the bar.
