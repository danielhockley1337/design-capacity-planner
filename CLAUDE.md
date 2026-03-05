# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A standalone, browser-based quarterly design capacity planning tool for the Dow Jones product design team. No build step, no dependencies, no backend.

## Running the Tool

Open `capacity-planner.html` directly in a browser — no server required.

## Architecture

Everything lives in a single file: `capacity-planner.html`. It is structured as:

1. **CSS** — all styles inline in `<style>`. Layout is a two-column CSS grid (initiatives list left, team details right).
2. **HTML** — two portal `<div>`s at the top (`#sizeDropdownPortal`, `#rowMenuPortal`) for dropdowns that must render above all rows, followed by the page layout.
3. **JavaScript** — a single `<script>` block at the bottom with:
   - `state` object (`{ quarter, initiatives[], team[] }`) as the single source of truth
   - `save()` / `load()` for `localStorage` persistence (key: `capacityPlanner_v1`)
   - `render()` calls `renderInitiatives()` and `renderTeam()` — full re-render on every state change
   - `calcScope()` computes the capacity cutoff index by walking initiatives in rank order and summing hours until `totalAvailableHours()` is exceeded

## Key Constants

```js
SIZE_HOURS  = { S: 80, M: 240, L: 480 }   // hours per t-shirt size
LEVEL_DEFAULTS = { junior: 300, mid: 360, senior: 400, lead: 320 }  // quarterly hours by level
```

> Note: the PRD specifies S=8, M=16, L=40 — the implementation uses 10x those values. Confirm before changing.

## Dropdown Pattern

Both the size picker and the row `···` menu use a **portal pattern**: a single fixed-position `<div>` appended to `<body>` is reused for all dropdowns. On click, the portal is populated with the relevant options and positioned via `getBoundingClientRect()`. This avoids z-index/overflow clipping issues from parent rows. A global `document.addEventListener('click', ...)` closes both portals.

## State & Persistence

All mutations follow the same pattern: modify `state`, call `save()`, call `render()`. There is no partial update — `render()` always rebuilds the full initiatives list and team list from scratch.

## PRD

`PRD-quarterly-design-capacity-planning-tool.md` is the source of truth for requirements. Keep it in sync when behaviour changes.
