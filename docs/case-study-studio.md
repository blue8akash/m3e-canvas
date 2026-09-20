# Case study studio

How we author Growth.Design-style case studies in the browser.

This studio is **only** for case studies. It is not a Material screen editor.

The kit of pieces you can drop on a slide lives in:

`Growth Design Case/Case Studies/SLIDE_COMPONENTS_CATALOG.md`

---

## The problem

You drag pieces on a **canvas**. That feels like a drawing tool.

When someone watches the story, **Reveal.js** must run it. Reveal owns:

- Right / Left arrows
- Pieces that appear on a later click (fragments)
- A phone that morphs to the next screen (auto-animate)
- Window arrows and the progress bar

If the canvas is one layout engine and Reveal is another, the two will drift. A bubble that looks right while you drag will sit wrong in the show.

**Rule:** one document. Two modes. One renderer.

---

## One document, two modes

```text
Story JSON
   (slides → blocks → motion)
        │
        ▼
  Block renderer
  (real .sl-block HTML + case-study CSS)
        │
        ├── EDIT   canvas + drag handles
        │          Reveal is off
        │
        └── PLAY   same HTML inside Reveal.js
                   Reveal is on
```

JSON is what we save.

The renderer is the only thing that turns a block into HTML.

Export writes that HTML to an `index.html`, same shape as the existing offline decks.

---

## The board

Each slide is **1280 × 720**.

That matches the decks we already have:

```js
Reveal.initialize({
  width: 1280,
  height: 720,
  margin: 0.07,
  transition: "none",
  fragmentInURL: true,
});
```

Two layers:

| Layer | What it is | Dragged? |
|---|---|---|
| **Room** | Slide background: color, photo, dimmer | No. Set on the slide. |
| **Blocks** | Everything on top: phone, balloon, face, card | Yes. |

The room is `data-background-color`, `data-background-image`, and `data-background-opacity` on the `<section>`. One photo often stays for many slides.

Reveal’s own arrows and progress bar sit on the **window**, not on the 1280 board. Do not put them on the canvas.

---

## The JSON shape

```text
Story
 └── Slide[]          room + milestone + auto-animate
      └── Block[]     type, x, y, w, h, class, motion
           └── Group  several blocks locked as one
```

A block is the same idea as `.sl-block` in the current HTML.

Minimum fields:

```json
{
  "id": "balloon-1",
  "type": "speechBalloon",
  "x": 160,
  "y": 520,
  "w": 320,
  "h": 120,
  "text": "Let’s get it…",
  "locked": false,
  "fragment": { "index": 2, "effect": "fade-in" }
}
```

`id` must stay stable. Reveal auto-animate matches the same id across slides. That is how a locked phone can grow or slide while the screenshot inside changes.

---

## Block types (the palette)

These are the catalog pieces, grouped the way the editor should show them.

**Shared kit** (ship once, reuse on every story):

| Palette group | What you drop |
|---|---|
| Device | iPhone / laptop frame. Screenshot or video sits **inside**. |
| Character | Dan or LX. HUD circle (story) or jumbo body (cover). Emotion swap. |
| Talk | White speech balloon, or yellow / blue caption |
| HUD | Psych meter, HUD face |
| Teaching | Callout card (`#UX Pro Tip`, `#Psychology Insight`, …) |
| Cursor | Tap hand (loop or still), `{tap}` / `{click}` text |
| Cover | Title, duration, keyboard PNG + looping hand + caption |
| Stickers | Post-it, green ✓, red ✕, badges, highlight |
| Outro | Offer slide, secret slide, golden check |

**Per story** (you import, not a shared widget):

- Room photo
- Product screenshots, GIFs, MP4s
- Exact copy
- Rare one-offs (kiosk body, swipe, wheel)

**On every block**, not a separate widget:

- **Fragment** — show on a later Right-arrow
- **Locked** — keep position across slides
- **Motion** — loop, click-step, arrive-once, or the file itself looping

The cover keyboard is three blocks grouped: still keys PNG, looping hand on the green key, yellow caption. Do not flatten that into one screenshot.

---

## The renderer

One function: `block → HTML`.

It must emit the same classes the live decks use, for example:

- `contained--iphone-14-pro--portrait`
- `speech-balloon-container`
- `hud__callout hud__callout--psychology hud__callout--animated`
- `hud__cursor--mobile hud__cursor--animated`
- `conversation__caption`
- `fragment fade-in`

It must load `slides-theme__case-study.css` and the GrowthComic / Inter fonts.

Edit and Play both call this renderer. Edit only adds selection chrome **around** that HTML. It does not redraw the balloon in a second style.

---

## Edit mode

Reveal is off. Keyboard is the editor’s.

The canvas is the 1280×720 board, scaled to fit the window.

You can:

- Drag a piece from the palette onto the slide
- Move, resize, and group
- Set room (photo, color, dim)
- Set fragment index and effect on a block
- Lock a device so it keeps the same id on the next slide

Loops still run in edit (the tap hand, meter pulse). Those are CSS. They do not need Reveal.

### Beat scrubber

A slide has steps (beats). Step 0 is the slide as it opens. Each later step is one Right-arrow.

The scrubber shows step 0, 1, 2… on the current slide. At step 2 you see what Reveal would show after two clicks. You still drag in that state.

Later beats can look faded when you are on an earlier step. That is an editor hint. Reveal does the real hide/show in Play.

---

## Play mode

We mount the **same** HTML:

```html
<div class="reveal">
  <div class="slides">
    <section
      data-background-image="office.jpg"
      data-background-opacity="0.47"
      data-auto-animate
      data-auto-animate-duration="0.3">
      <!-- blocks from the renderer -->
    </section>
  </div>
</div>
```

Then:

```js
Reveal.initialize({
  width: 1280,
  height: 720,
  margin: 0.07,
  hash: true,
  controls: true,
  progress: true,
  fragmentInURL: true,
  transition: "none",
  backgroundTransition: "none",
  center: false,
});
```

Now Right-arrow is Reveal’s. Fragments, auto-animate, window chevrons, and the progress bar all work as they do in the current runners.

Edit handles are gone.

---

## Who owns what

| Job | Owner |
|---|---|
| Position, size, text, image, CSS class | The block (JSON) |
| Room photo / color / dimmer | The slide (`<section>` data attributes) |
| Looping tap hand, meter pulse | CSS on the block |
| Video / GIF | The media file (`loop`, autoplay) |
| “Next click shows this balloon” | Reveal fragment on that block |
| Phone morphs to the next screen | Reveal auto-animate + stable `id` + `locked` |
| Keyboard, window arrows, progress bar | Reveal window chrome |
| Drag, resize, selection, palette | The editor, Edit mode only |

---

## From drag to show (one example)

1. You drop a speech balloon. We store the JSON above.

2. **Edit.** The canvas shows it. If `fragment.index` is 2, the scrubber at step 0 can ghost it. Reveal is not listening.

3. **Play.** The renderer writes:

```html
<div class="sl-block fragment fade-in"
     data-fragment-index="2"
     data-block-id="balloon-1"
     style="left:160px;top:520px;width:320px;height:120px">
  <!-- speech balloon markup -->
</div>
```

Reveal shows it on the third click.

4. **Export.** That tree is the offline `index.html`. No second layout pass.

---

## What we will not do

- A React-only preview that “looks like” the story, plus a separate Reveal export. Those two will drift.
- Drive the editor with Reveal’s keyboard. Then every drag fights slide changes.
- Fake fragments with Motion in Edit and hope Reveal matches later. Fragments are data on the block. The editor only **shows** them. Reveal **runs** them.
- Put Reveal chevrons on the 1280 board. Those belong on the window.
- Flatten the cover keyboard into one PNG. The hand must keep looping.

---

## Export

Playable output is a folder like the current `Code of Same Case Study/`:

```text
index.html          Reveal runner
*_files/            theme CSS, reveal.js, fonts, media
fonts/              GrowthComic + Inter
```

The HTML is the renderer output plus `Reveal.initialize`. It should open offline.

---

## Source of truth for pieces

Do not invent new widgets without checking the catalog first.

| Need | Where |
|---|---|
| What you can drop | `SLIDE_COMPONENTS_CATALOG.md` |
| How a live slide is marked up | any `Code of Same Case Study/index.html` (search `sl-block`) |
| Look | `slides-theme__case-study.css` |
| Sorted assets | each case: `App Screenshots/`, `Character Emotions/`, `UI Graphics/`, `UI Callout Icons/`, `UI Cursors/` |

---

## Status

This file is the design for the studio. It is not implemented in this repo yet.

The current app still sketches Material 3 screens. This studio would clone the **editor pattern** (canvas, palette, inspector, JSON document) and replace the domain with slides and catalog blocks.
