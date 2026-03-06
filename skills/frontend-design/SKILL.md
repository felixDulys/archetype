---
name: frontend-design
description: "Create distinctive, production-grade frontend interfaces with high design quality. Use this skill when the user asks to build web components, pages, or applications. Generates creative, polished code that avoids generic AI aesthetics."
allowed-tools:
  - Read
  - Bash
  - Glob
  - Grep
  - Edit
  - Write
  - Task
---

# Frontend Design — UI Consistency Guide

Reference guide for building and reviewing UI in the Pelikanas app. All agents building or reviewing frontend code should follow these rules.

## Design Philosophy

**Minimal and clean by default, with drill-in for detail.** Pages start sparse — drawers closed, accordions collapsed, only essential columns visible. Users opt into complexity; they should never be overwhelmed on first load.

## Language

The app supports two independent language settings:

1. **Grammar labels** (existing) — toggles grammatical terms (case names, POS, tense) between English and Lithuanian
2. **UI language** (planned) — toggles all UI chrome (column headers, button text, tab names, empty states, tooltips) between English and Lithuanian

Both settings are independent. A user might want English UI with Lithuanian grammar terms, or full Lithuanian everywhere.

**Rule:** Every user-facing string (header, label, button, placeholder, empty state, tooltip) must be translatable. When building new UI, use a keyed string approach that can be swapped by the UI language setting. Never hardcode English-only labels in places that should respect the setting.

---

## Tables

### Column Rules

| Rule | Detail |
|------|--------|
| **Max visible columns** | 3–4. If you need more, the page design needs rethinking. |
| **Justify each column** | Every column must earn its place. Ask: "Does the user need this at a glance?" If not, it belongs in a drill-in view. |
| **Header naming** | 1–2 words max. Use intuitive, plain names: "Word", "Type", "Status" — not "Verification Status" or "Stress Pattern Index". |
| **Header casing** | Title Case for English headers. |
| **Alignment** | Text left-aligned. Numbers right-aligned. Status indicators centered. |

### Spacing & Density

Tables use the project's spacing variables and respect the density setting (`compact` / `normal` / `spacious`).

**Reference density** (the /verify page, which the user considers well-spaced):

| Element | Vertical | Horizontal |
|---------|----------|------------|
| Entry table cells | `var(--lentele-spacing-sm)` (0.5rem) | `var(--lentele-spacing-md)` (1rem) |
| Detail/forms table cells | `var(--lentele-spacing-xs)` (0.25rem) | `var(--lentele-spacing-md)` (1rem) |

**Rules:**
- Always use `--lentele-spacing-*` variables for padding — never hardcoded `px` or `rem` values. This ensures the density multiplier applies.
- Summary/entry tables: `padding: var(--lentele-spacing-sm) var(--lentele-spacing-md)`
- Dense detail tables: `padding: var(--lentele-spacing-xs) var(--lentele-spacing-md)`
- Font size for table content: `var(--lentele-font-size-sm)` (0.875rem)

### Row Interactions

| Pattern | When |
|---------|------|
| **Navigate to detail page** | Summary/listing pages where rows represent entries |
| **Expand inline** | Detail pages where rows show forms/variants of a single entry |
| **Side drawer** | When comparing the detail to the list context is important |

- **Hover actions:** Action buttons (edit, delete, verify) appear on row hover only. No permanent "Actions" column.
- **Hover effect:** `box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08)` + subtle background color shift. Never use `transform` for hover lift (Chrome GPU rule).
- **Cursor:** `cursor: pointer` on clickable rows.

### Sorting & Filtering

- **Sortable headers:** Clickable column headers with a small arrow indicator for sort direction. All tables should support sorting on at least the primary column.
- **Filter bar:** For complex pages with multiple filter dimensions, add a filter bar above the table with chips or compact dropdowns.
- **Simple pages:** Sortable headers only, no filter bar needed.

### Data Overflow (columns > 4)

When a table's data exceeds the 3–4 column limit, use these strategies (can be combined):

- **Tooltips:** Hover over a cell to see supplementary detail
- **Expandable rows:** Click to expand an inline detail section below the row
- **Drill-in navigation:** Click navigates to a full detail view (page or drawer)
- **Column chooser:** Let the user toggle which columns are visible

> **Note:** The best pattern for each context is still being evaluated. When in doubt, default to drill-in navigation for summary pages and expandable rows for detail pages.

### Loading States

- **Use skeleton rows** that match the table structure (correct number of columns, approximate row height). Gray placeholder blocks that pulse with an opacity animation.
- Never use a spinner overlay or bare "Loading..." text for tables.
- Skeleton animation: opacity-only (no `transform` — Chrome GPU rule).

### Empty States

- **Always use the `EmptyState` component** from `@pelikanas/lentele`.
- Props: `{ message: string }` — provide a helpful, action-oriented message.
- Good: `"No entries match your search"`, `"Add your first vocabulary set"`
- Bad: `"No data"`, `"Empty"`

### Color & Status Indicators

- **Subtle pastel backgrounds** for row-level status (e.g., light green for verified, light amber for pending).
- Use the project's pastel variables: `var(--lentele-pastel-green)`, etc.
- Never use bold/saturated colors for table row backgrounds.
- Text within status rows should remain the standard text color — don't color the text to match the background.

---

## Cards

### When to Use

- Use cards for items that have a visual identity (vocabulary sets, quiz categories, dashboard stats).
- Use tables for homogeneous lists of similar items (entries, forms, queue items).

### Card Rules

| Rule | Detail |
|------|--------|
| **Content limit** | 3–5 pieces of information max per card. Title + 2–3 stats + 1 action. |
| **Sizing** | Use `ItemCard` from `@pelikanas/lentele` when the pattern fits. |
| **Hover** | `box-shadow` change only. No `transform` (Chrome GPU rule). |
| **Click target** | Entire card is clickable, not just a button within it. |

---

## Forms

### Layout

- Single column for simple forms (< 5 fields).
- Two columns for wider forms, but group related fields together.
- Labels above inputs (not inline/floating).

### Input Rules

| Rule | Detail |
|------|--------|
| **Placeholders** | Use actual Lithuanian characters, never Unicode escapes. Provide example text, not instructions. |
| **Validation** | Inline below the field, in red. Show on blur, not on every keystroke. |
| **Required indicator** | Small asterisk after label. |
| **Submit button** | Disabled until form is valid. Clear label: "Save", "Add Entry" — not "Submit". |

---

## Modals & Drawers

### Drawers

- **Default state:** Closed on page load. User opens explicitly.
- **Width:** 400–500px for detail drawers. Never full-width.
- **Close:** Click outside, Escape key, or explicit close button.
- **Animation:** Opacity fade only. No `transform: translateX()` slide-in (Chrome GPU rule).

### Modals

- **Use sparingly.** Only for confirmations and destructive actions.
- **Always provide:** Title, body, Cancel + Confirm buttons.
- **Destructive confirms:** Red confirm button, require explicit action name (e.g., "Delete Entry" not just "OK").

---

## Navigation & Page Structure

### Page Shell

- All pages use `PageShell` from `@pelikanas/lentele`.
- Consistent header area with page title and optional breadcrumb.

### Drill-In Pattern

```
Summary page (table, 3-4 cols)
  → Click row → Detail page OR drawer
    → Detail page has its own tables/sections with full info
```

Drawers stay open until explicitly closed. If the user navigates away, the drawer closes.

---

## Shared Components (always use these)

| Component | Package | Use For |
|-----------|---------|---------|
| `PageShell` | lentele | Page wrapper with title |
| `ItemGrid` | lentele | Card grid layouts |
| `ItemCard` | lentele | Individual cards |
| `SearchBox` | lentele | Search inputs |
| `StatsCounter` | lentele | Numeric stat displays |
| `ErrorBanner` | lentele | Error display (never custom) |
| `EmptyState` | lentele | No-data states (never custom) |
| `StatsRow` | lentele | Row of stats |

**Rule:** Before building any UI element, check if a shared primitive exists. Reimplementing a pattern that has a shared component is a review failure.

---

## CSS Rules (from CLAUDE.md, repeated here for completeness)

1. **Never `transition: all`** — list specific properties
2. **Never `transform` for hover/active** — use `box-shadow`
3. **Never `transform` in `@keyframes`** — opacity-only animations
4. **Never `will-change`** unless profiling proves need
5. **Never `background` shorthand** when sibling classes use `background-image`
6. **Spinners only exception** for `transform: rotate()`

---

## Accessibility

| Rule | Detail |
|------|--------|
| **Interactive elements** | All buttons, links, and controls must have accessible labels |
| **Keyboard navigation** | Tab through all interactive elements. Enter activates. Escape closes modals/drawers. |
| **Focus indicators** | Visible focus rings on all interactive elements |
| **Color contrast** | Never rely on color alone to convey meaning — pair with text or icons |
| **Hover actions** | Elements revealed on hover must also be accessible via keyboard (e.g., focus on row reveals actions) |

---

## Review Checklist (for frontend-reviewer agent)

When reviewing any UI component, check:

- [ ] Max 3–4 columns in tables
- [ ] Column headers are 1–2 words, intuitive names
- [ ] Uses `--lentele-spacing-*` variables (not hardcoded spacing)
- [ ] Hover actions, not permanent action columns
- [ ] Skeleton loading states for tables
- [ ] `EmptyState` component for no-data states
- [ ] Subtle pastel status indicators (not bold colors)
- [ ] All shared primitives used (no reimplementation)
- [ ] Drawers default closed on page load
- [ ] CSS rules followed (no transform hover, no transition all)
- [ ] Lithuanian characters used directly (no Unicode escapes)
- [ ] Labels translatable for dual-language support
- [ ] Keyboard accessible (tab, enter, escape)
