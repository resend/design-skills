# Complementary information (tooltip, placeholder, drawer, etc.)

> **Status: TODO.** Notion source has bullets but no finalised guidance. Treat the points below as drafts and escalate to `@design` for anything binding.

## Draft notes from Notion

- Inconsistency is the main issue today — there is no single rule for explaining something extra to the user.
- Tooltips shouldn't be too long; if they need to be, link to documentation instead.
- Do not use a label beneath text fields.
- A documentation component is being considered to avoid cluttering simple components (e.g. topics).
- Use the toggle component for boolean actions, with an inline description.
- Use a drawer when the supplementary content is long.

## Open questions

- A consistent decision rule for picking between tooltip, placeholder, inline description, sidenote, and drawer.
- Whether the documentation component will become a primitive in `src/ui/`.

Until this graduates, prefer the existing primitives (`Tooltip`, `Drawer`, `TextField.Slot`) and keep helper copy short.
