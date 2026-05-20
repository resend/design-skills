# Expose debuggable data

> **Status: TODO.** Notion source has bullets but no finalised guidance. Treat the points below as drafts and escalate to `@design` for anything binding.

## Principle (draft)

Resend's users are developers. When something fails or behaves unexpectedly, give them the raw signals they'd otherwise have to dig into logs to find.

## Draft notes from Notion

- **Email debounce data** → show the reason plus the raw underlying data.
- **Contact activity** → show the source of each event.
- **Webhooks** → show the original error code, the number of attempts, and the raw payload.

## How to apply this

- On a resource's detail or activity view, surface: status, timestamp, reason, source, and a way to inspect the raw payload that produced the event.
- Prefer "show the JSON" over "hide the JSON behind a support ticket."

## Open questions

- A consistent visual treatment for raw payloads (collapsible JSON viewer? a "Raw" tab?).
- Whether sensitive fields should ever be redacted in this view.

## Related

- [`table-required-fields`](table-required-fields.md) — debug fields belong in the detail page, not the main table.
