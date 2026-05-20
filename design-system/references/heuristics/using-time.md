# Using time

> **Status: TODO.** Notion source has bullets but no finalised guidance. Treat the points below as drafts and escalate to `@design` for anything binding.

## Draft notes from Notion

- Default to **relative time**.
  - Avoid "about…" and "less than…" — they read as vague.
  - Use short mode for minutes, seconds, hours — e.g. `13m ago`, `2h ago`.
- Always provide the **accurate exact date and time in UTC** in a tooltip on hover, so users can recover the precise moment when they need it.

## Open questions

- The exact threshold where relative time switches to absolute (e.g. older than 7 days).
- Whether timezone conversion happens on the client or server.
- Format for absolute times in tooltips (ISO 8601? localised?).

Until this graduates, prefer relative + UTC tooltip combinations already seen in the dashboard.
