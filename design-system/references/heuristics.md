# Resend Heuristics

Guiding principles for UX decisions across the Resend dashboard. They help maintain a consistent mental model and resolve recurring "which pattern fits here?" questions.

> **Guidelines, not strict rules.** Heuristics describe how Resend usually decides, not what every screen must do. Exceptions are expected. When a heuristic doesn't obviously apply, or a screen has a strong reason to break one, escalate to `@design` instead of forcing the rule.

Source of truth: [Resend Heuristics (Notion)](https://www.notion.so/resend/Resend-Heuristics-183c40d6c4ef8094a6d7c90afdf1866a). This directory mirrors that doc so the heuristics are available inside Claude Code sessions; the Notion page wins if they ever diverge.

## When to load these

Load the relevant heuristic file when:
- Choosing between two UI patterns (dialog vs stepper, hide vs disable, alert vs banner, etc.)
- Reviewing a screen and trying to explain *why* it feels off, beyond token/component violations
- Writing a `design-audit` finding that depends on judgment, not a grep rule — cite the heuristic as `design_ref`
- A reviewer asks "what's the Resend convention for X?" and X is a decision, not a component

## Index

| Heuristic | Decision it helps with |
|-----------|------------------------|
| [dialog-stepper-fullscreen-drawer](heuristics/dialog-stepper-fullscreen-drawer.md) | Which container pattern fits a task |
| [disable-vs-hide](heuristics/disable-vs-hide.md) | When to disable an action vs remove it |
| [api-first](heuristics/api-first.md) | How API and dashboard capabilities relate |
| [error-and-alert-communication](heuristics/error-and-alert-communication.md) | Inline error vs alert vs notification vs page error |
| [table-required-fields](heuristics/table-required-fields.md) | Which columns belong in a table's main view |
| [button-appearance](heuristics/button-appearance.md) | Primary/secondary/destructive/ghost button choices, Button vs IconButton |
| [complementary-information](heuristics/complementary-information.md) | Tooltip vs placeholder vs label vs drawer for helper content |
| [affordance](heuristics/affordance.md) | Whether an element looks interactive when it shouldn't (or vice versa) |
| [using-time](heuristics/using-time.md) | Relative vs absolute time, formats, thresholds |
| [friendly-names-over-ids](heuristics/friendly-names-over-ids.md) | When to show aliases instead of raw IDs |
| [expose-debuggable-data](heuristics/expose-debuggable-data.md) | Surfacing raw payloads, error codes, sources |
| [tag-colors-for-status](heuristics/tag-colors-for-status.md) | Reserving Tag / Badge colours for status, not decoration |

Each row links to a file under `heuristics/`. Files land via separate PRs — links resolve as those merge.

## How heuristics relate to other parts of the design system

- **`design-system/references/components.md`** answers *how* to build a primitive — props, sizes, slots.
- **`design-system/references/patterns/`** documents *compositions* of primitives that recur ≥ 3 times.
- **Heuristics (here)** answer *which* of several valid options to pick when more than one composition is reasonable.

A heuristic is not the same as a documented pattern. A pattern is a concrete composition you can copy-paste; a heuristic is the reasoning that tells you which pattern (or primitive) belongs on this screen.

## Adding or updating a heuristic

1. Update the Notion source first (or get sign-off from `@design`).
2. Mirror the change into the matching file under `heuristics/`. Keep the file short — one screen of prose, plus a "Best practices" / "Anti-patterns" list where useful.
3. If the heuristic introduces a new judgment call the audit should flag, add a sentence in `design-audit/references/rubric.md` (Category 6) pointing to the file.
