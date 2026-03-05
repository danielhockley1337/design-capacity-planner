# PRD: Quarterly Design Capacity Planning Tool

**Status:** Draft
**Author:** Daniel Hockley
**Last Updated:** 2026-03-05

---

## Problem

Every quarter, the product design team must work with marketing stakeholders to force-rank design initiatives and determine what can realistically be delivered given team capacity. This process currently lacks a dedicated tool, making it difficult to visualise capacity constraints in real time during planning sessions and to reach shared alignment on what is in or out of scope.

---

## Goal

A lightweight, browser-based tool that lets the team collaboratively force-rank design initiatives, automatically calculate cumulative hour commitments against available team capacity, and produce a clear visual cutoff between what is in scope and out of scope for the quarter.

---

## Users

- Product design team (own and maintain team roster and capacity)
- Marketing stakeholders (participate in initiative prioritisation during planning sessions)
- VP of Product (facilitates and approves final scope)

---

## Sections

### 1. Initiatives

The primary working area. A dynamic, drag-and-drop ordered list of design initiatives for the quarter.

**Each initiative contains:**

| Field | Description |
|---|---|
| Rank | Determined by drag-and-drop position (1 = highest priority) |
| Name | Free-text title of the initiative |
| Size | T-shirt size: **S**, **M**, or **L** |
| Hours | Automatically derived from size (see Size Definitions below) |
| Cumulative Hours | Running total of hours from rank 1 down to this initiative |

**Size Definitions (fixed):**

| Size | Hours |
|---|---|
| S | 80 |
| M | 240 |
| L | 480 |

**Capacity cutoff line:**

The tool automatically calculates total available hours from the Team Details section and renders a visible horizontal divider in the initiative list at the point where cumulative hours first exceed total capacity. All initiatives above the line are **In Scope**; all initiatives at or below the line are **Out of Scope**. The line updates in real time as initiatives are reordered, added, removed, or resized.

**Default state:**

On first load (no `localStorage` data), the tool pre-populates a set of named initiatives drawn from real Dow Jones design work. All default entries are valid initiative names — no placeholder or junk data is included.

**Initiative management:**

- Add a new initiative (name + size) via an inline form at the top of the list
- Reorder by dragging
- Each row has a `···` menu (visible on hover) with the following options:
  - **Send to top** — moves the initiative to rank 1
  - **Send to bottom** — moves the initiative to the last position
  - **Delete** — removes the initiative

**Size selection:**

Clicking the size badge on an initiative opens a small dropdown showing all three size options (S, M, L) with their hour values. A second click selects the new size. This requires a deliberate two-click action to prevent accidental size changes during planning sessions.

---

### 2. Capacity

A list of named hour allocations that collectively define total available team capacity for the quarter. The section header is labelled **"Capacity"** in the UI.

**Allocation record:**

| Field | Description |
|---|---|
| Name | Free-text label for the allocation (e.g. a designer's name, a shared pool) |
| Quarterly Hours | Editable numeric value; defaults to 2,000 hours on add |

There are no levels or role-based defaults. Each allocation is a simple name + hours pair. The default on add is 2,000 hours; this is editable directly in the roster.

**Capacity management:**

- Add an allocation (name only) via an inline form; hours default to 2,000 and can be edited immediately
- Remove any allocation via a delete button on each row
- Edit any allocation's quarterly hours directly in the roster

**Lock / unlock:**

The Capacity section has a password-protected lock. The section renders locked by default on page load (add, remove, and hours-edit controls are disabled). Clicking the lock icon prompts for a password; on success the section unlocks for editing. Clicking the icon again re-locks without a password prompt. The lock state is session-only — it resets to locked on every page load.

**Capacity summary** (displayed at the top of the Initiatives card, above the add form):

| Metric | Value |
|---|---|
| Available hrs | Sum of all allocations' quarterly hours |
| Committed hrs | Sum of hours for all In Scope initiatives |
| Remaining hrs | Available − Committed (turns red if negative) |

All three values update in real time. The counter in the Capacity card header shows the number of allocations (e.g. "3 allocations").

---

## Behaviour & Constraints

- **No backend required.** The tool runs entirely in the browser. State persists via `localStorage` so data survives page refreshes.
- **Real-time updates.** All calculations (cumulative hours, cutoff line, capacity summary) recalculate on every user interaction.
- **Cutoff is automatic.** There is no manual in/out-of-scope toggle. Rank determines scope; capacity determines the cutoff.
- **Size is fixed-value.** S/M/L map to fixed hour values and are not user-configurable in this version.

---

## Out of Scope (v1)

- Multi-quarter planning
- Assignment of initiatives to specific designers
- Export to PDF or spreadsheet
- Authentication or multi-user collaboration
- Configurable size-to-hour mappings

---

## Quarter History

Quarter history is **not yet implemented**. The quarter label input in the page header saves to `localStorage` alongside the rest of the state, but there is no archiving, history selector, or cross-quarter comparison in the current version.

## Open Questions

- ~~Should the tool support a quarter label (e.g. "Q2 2026") for context during stakeholder sessions?~~ *(Implemented)*
- Should there be a way to archive or compare previous quarters? *(Not yet implemented — previously marked resolved in error)*
- ~~Should initiatives support an optional description or notes field?~~ *(Resolved — not needed)*
- Should level-based defaults be re-introduced to the Capacity section, or is the simplified flat-allocation model sufficient?
