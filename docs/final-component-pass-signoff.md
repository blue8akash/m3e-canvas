# Sign-off: final component pass

**Date:** 2026-09-20  
**Brief:** [grok-final-component-pass-brief.md](./grok-final-component-pass-brief.md)  
**Catalog:** `Growth Design Case/Case Studies/SLIDE_COMPONENTS_CATALOG.md`  
**Studio:** `d:\MAJOR-NODES\PRODUCT-RD\Growth-Design-Engine\studio`

UX calm (`audit/21`–`23`) is accepted. This note is only about **what to add** so the kit can author the 53 decks **without** a new cockpit.

---

## 1. Sign-off on the four asks

| Ask | Sign-off | How |
|---|---|---|
| **Callout teaching cards** | **Yes. Highest priority.** | One droppable **block**, many variants. Not a new slide archetype. |
| **Jumbo actor body** | **Yes.** | Same character as HUD, **size/pose**. One Actors tile. |
| **Thought / radio balloons** | **Yes, as a style on the existing balloon.** | Not a new palette tile. |
| **Video inside devices** | **Yes, as media in the glass.** | Not a palette tile. Gap table said “complete”; `studio/src` has **no** `<video>`. Do this. |
| **Editable sticky copy** | **Yes.** | Inspector + double-click. Merge the Labor-specific post-its. |
| **Customer journey graph** | **No, not this pass.** | Rare (McDonald's). Would clutter the palette. Same for FITB, vote meter, swipe, wheel, invisiblock. |

That is enough to recreate the **shared kit** of any case study. Per-deck screens, room photos, and one-off widgets stay in harvest (`sandbox/assets/`), not as extra tiles.

---

## 2. Stickers vs archetypes

**Do not add archetypes.**  
`callout_pro_tip` as a *slide type* is why a card cannot sit next to a phone. In the decks, a callout is an `.sl-block` **on** a story slide (`hud__callout` beside the device). Cover / phone / observation / outro stay as **filmstrip starters** (presets), not as the only place a piece can live.

**Use the sticker/block list you already have** (`SlideSticker[]`), with a richer `type` — same path as extra phones and extra balloons.

```ts
type: 'postit' | 'sticker_img' | 'speech_balloon' | 'avatar'
    | 'device_phone' | 'device_laptop'
    | 'callout_card'      // NEW
    | 'jumbo_avatar'      // NEW (or avatar.scale = 'jumbo')
```

| Piece | Model | Why |
|---|---|---|
| Callout card | `SlideSticker` `type: 'callout_card'` | Many per slide; drop on any archetype; `reveal_beat`; stack z ~14 |
| Jumbo body | `SlideSticker` `type: 'jumbo_avatar'` **or** `avatar` + `scale: 'hud' \| 'jumbo'` | Cover can have jumbo; later slides have HUD. They are not the same node. |
| Thought / radio | Field on balloon: `balloon_style: 'speech' \| 'thought' \| 'radio'` | Catalog: same widget, `speech-balloon__text--thought` / `--radio` |
| Video | `screen_img` / `img_src` ending in `.mp4` / `.webm` | Render `<video autoPlay loop muted playsInline>` in the bezel. No `type: 'video'` |
| Sticky text | `sticker.text` already exists | Show it in Inspector; double-click on canvas |
| `callout_pro_tip` archetype | Keep as **preset** | Filmstrip “+ Pro Tip” = empty room + one `callout_card` already placed. The piece is still a sticker. |

Do **not** add `tip_title` / `tip_body` slide fields for the new cards. Those keep you at one card per slide.

---

## 3. Keep the palette calm (one tile per verb)

Palette already has five groups. Add **two tiles**, not seven callout flavors.

| Group | Tiles |
|---|---|
| Device Frames | Phone, Laptop *(unchanged)* |
| Comic Story | Speech Balloon, Yellow Caption |
| Actors | HUD Avatar, **Jumbo Body**, Meter, Tap Hand |
| Teaching | **Callout Card** *(one tile)* |
| Notes | Sticky Note, Gold Check, **Red X** *(pair with check; catalog has both)* |
| Cover | Keyboard Hint |

**Inspector, not palette:**

- Callout → variant dropdown: UX, Psychology, Pro Tip, UI, Copy, Data, Experiment, Ethics, Bonus, … (catalog §8). Default title `#UX Pro Tip` etc. Body + `hud__callout__source`.
- Balloon → Speech / Thought / Radio.
- Caption → yellow / blue (`conversation__caption--alternate`).
- Tap hand → mobile / desktop cursor (`hud__cursor--desktop`).
- Device → if file is video, play in glass. Frame selector already exists.
- Jumbo → Dan / LX + pose from harvested full-body files.

**Remove from palette this pass:** `postit_bias` and `postit_roi` as separate kinds. They are Labor Perception stickers, not a general kit. One Sticky Note + optional template in Inspector.

Do not add tiles for: journey graph, FITB, vote meter, swipe, wheel, duration clock, cover pill, window chrome, rotate-phone GIF.

---

## 4. Catalog classes (do not invent a second look)

| Piece | Classes / motion |
|---|---|
| Callout | `hud__callout` + `hud__callout--psychology` (etc.) + `hud__callout--animated` (`rotate-springish`) |
| Jumbo | `jumbo-avatar` / `jumbo-deja-la` (cover). HUD stays `hud__avatar__*` |
| Thought | existing balloon + `speech-balloon__text--thought` |
| Radio | `speech-balloon__text--radio` |
| Video | same `contained--iphone-*` / `contained--desktop-*`; media is `<video loop>` |
| Sticky | `.postit` / `--blue` / `--red` / `--green` |

Compiler must emit these, not a custom Studio card.

---

## 5. Calm inspector (same rule as balloon)

Select a callout → **only** that card (variant, title, body, source, beat, remove).  
Select jumbo → character, pose, X/Y.  
Empty click → short Slide Settings (room + stack).  
Do not put all variants as always-visible tiles.

Default size (catalog-ish): callout ~420×280, right of phone (`left` ~820). Jumbo full-body, cover left/center. Thought balloon uses the same box as speech.

---

## 6. Out of this pass (still not “100% of every HTML widget”)

Shared kit **yes**. Every interactive extra **no**.

Skip until a real deck needs them in Studio:

- `#customer-journey-graph`
- Fill-in-the-blank, vote meter, swipe, wheel
- `gd-invisiblock`, screen overlay, highlight marker
- Chapter milestone bar, outro offer slots as live forms
- Window Reveal chrome

Harvest already has the PNGs. Authors can drop `sticker_img` for a one-off.

---

## 7. Done when

1. Drop **one** Callout Card on a phone slide; switch variant to Psychology; Present shows `hud__callout--psychology` with spring-in; phone still there.
2. Cover has Jumbo Dan; a story slide has HUD circle; both from Actors, not a new archetype.
3. Balloon inspector: Speech / Thought / Radio. No extra palette tile.
4. Drop an `.mp4` on a phone; glass plays looped muted video. **Edit already does this** (`SlideCanvas.tsx`). Fix compiler only if Present still emits `<img>`.
5. Sticky Inspector edits copy. Bias/ROI are not separate left-rail kinds.
6. Palette still ~12–14 tiles. Balloon-only inspector rule still holds. `audit/21`–`23` still true.
