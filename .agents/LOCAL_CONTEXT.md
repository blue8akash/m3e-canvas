# Local Context Tracker

> **Repository:** m3e-canvas
> **Last Updated:** 2026-09-20
> **Governing System:** [Context-Matrix](https://github.com/blue8akash/Context-Matrix)

## 1. Project Purpose
_What is the overarching goal of this repository?_
- Sketch Material 3 Expressive (M3E) screens in the browser and turn them into vibe-coding prompts. Upstream repository by `lnkiai` (`https://github.com/lnkiai/m3e-canvas`).

## 2. Current Focus
_What is Co-Ma (or the local agent) actively building or refactoring right now?_
- Sidebar/controller proposal: borrow pills/search/emotion popover; do not add a left Beats app or a third beat clock. Critique: `docs/studio-sidebars-controllers-review.md`.

## 3. Active Blockers
_What is stopping progress? What dependencies are missing?_
- None. Node dependencies can be installed with `npm install` when ready to run locally.

## 4. Recent Decisions
_What architectural or technical decisions were recently made that future agents must know?_
- Forked repository to `blue8akash/m3e-canvas` (`origin`), with `upstream` pointing to `lnkiai/m3e-canvas.git`.
- Pushed local onboarding commits and transcript documentation to `origin/main`.
- Deployed Context-Matrix Agent Development Kit (ADK) v9 and linked to Context-Matrix.
- Case-study studio (design only): one JSON document, one `.sl-block` renderer, two modes (Edit canvas / Play Reveal.js). Kit source is `SLIDE_COMPONENTS_CATALOG.md`. Written up in `docs/case-study-studio.md`. This studio is not the current M3E screen editor.
- Studio drag diagnosis (2026-09-20): m3e-canvas drags real DOM nodes; Studio puts a Reveal iframe + dashed overlay. Overlay `follow()` origin-delta was fixed in `Growth-Design-Engine/studio/src/lib/drag.ts`. The iframe editor is still the gap. Handoff: `docs/studio-canvas-handoff.md`.
- Studio layers plan rewritten (2026-09-20) from Growth.Design HTML: lock + auto-animate, fragments stay on beats, `sl-block-group` for keyboard only. No `inheritFrom`, no `appearBeat` on layers. Plan: Gemini `implementation_plan.md`. Review: `docs/studio-implementation-plan-review.md`.
