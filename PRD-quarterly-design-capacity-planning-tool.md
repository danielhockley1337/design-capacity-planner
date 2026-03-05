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

**Initiative management:**

- Add a new initiative (name + size) via an inline form at the top or bottom of the list
- Reorder by dragging
- Each row has a `···` menu (visible on hover) with the following options:
  - **Send to top** — moves the initiative to rank 1
  - **Send to bottom** — moves the initiative to the last position
  - **Delete** — removes the initiative

**Size selection:**

Clicking the size badge on an initiative opens a small dropdown showing all three size options (S, M, L) with their hour values. A second click selects the new size. This requires a deliberate two-click action to prevent accidental size changes during planning sessions.

---

### 2. Team Details

A roster of the product design team that drives total available capacity for the quarter.

**Team member record:**

| Field | Description |
|---|---|
| Name | Designer's name |
| Level | Junior / Mid / Senior / Lead |
| Quarterly Hours | Auto-populated from level defaults; editable per person |

**Default quarterly hours by level:**

| Level | Default Hours/Quarter |
|---|---|
| Junior | 300 |
| Mid | 360 |
| Senior | 400 |
| Lead | 320 |

> Lead hours are lower by default to account for non-IC responsibilities (rituals, mentoring, reviews). All defaults are editable per person.

**Team management:**

- Add a team member (name + level) via an inline form
- Remove any team member
- Edit any individual's quarterly hours directly in the roster

**Lock / unlock:**

The Team Details section has a password-protected lock. The section renders locked by default on page load (add, remove, and hours-edit controls are disabled). Clicking the lock icon prompts for a password; on success the section unlocks for editing. Clicking the icon again re-locks without a password prompt. The lock state is session-only — it resets to locked on every page load.

**Capacity summary** (displayed within or adjacent to Team Details):

| Metric | Value |
|---|---|
| Total Available Hours | Sum of all team members' quarterly hours |
| Hours Committed | Sum of hours for all In Scope initiatives |
| Hours Remaining | Total Available − Hours Committed |

All three values update in real time.

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

The tool supports saving and reloading previous quarters for reference.

- The current quarter's state is **automatically saved** to history when the quarter label is changed (e.g. typing "Q3 2026" archives the current state as the previous quarter)
- A **dropdown selector** in the page header allows loading any previously saved quarter
- Loaded past quarters are **editable**, not read-only

## Open Questions

- ~~Should the tool support a quarter label (e.g. "Q2 2026") for context during stakeholder sessions?~~ *(Implemented)*
- ~~Should there be a way to archive or compare previous quarters?~~ *(Resolved — see Quarter History above)*
- ~~Should initiatives support an optional description or notes field?~~ *(Resolved — not needed)*
