# Expose debuggable data

> Guideline, not a rule. Resend's users are developers — show them the raw signal whenever a UI summary loses information they'd otherwise need to dig out of logs.

## Principle

When something fails, behaves unexpectedly, or has a state the user didn't directly cause, show the underlying data alongside the human-friendly summary. The dashboard should be a faster path to the same answer the user would get by `curl`-ing the API or reading their server logs — not a more polished one that loses fidelity.

## What "debuggable data" usually means

For any event, status change, or error surfaced in the UI, expose at least:

- **What happened** — the human-readable label (`"Bounced"`, `"Delivery failed"`, `"Webhook attempt 3 of 5 failed"`).
- **Why** — the reason code or category the upstream system returned.
- **When** — relative time on the surface, exact UTC timestamp in the tooltip (see [`using-time`](using-time.md)).
- **Where it came from** — the source / provider / endpoint / IP, when the user could care.
- **The raw record** — the underlying JSON, headers, or payload, available without leaving the page.

## Apply it on these surfaces

- **Email events / debounce data** — show the bounce reason in plain language, and expose the raw provider response next to it. The user trying to debug a deliverability issue needs both.
- **Contact activity** — every event (open, click, bounce, complaint, unsubscribe) should name its source (which broadcast, which webhook, which API call) so the user can trace the trigger.
- **Webhooks** — for each attempt, show the response status, the original error code, the number of retries, the next-attempt time, and the full request + response payload. A webhook page that hides the payload is failing the only person who'd look at it.
- **API key / token usage** — show the calling IP, user-agent, and timestamp on recent activity so a user can confirm a key isn't compromised.
- **Background jobs** — surface the queue state, attempt count, and last error message; let the user re-trigger.

## How to surface it without cluttering the page

- **Summary first, raw second.** The row in a table is the summary; the detail view (or an inline collapsible) holds the raw payload.
- **`<code>` blocks for raw payloads** with copy-to-clipboard. Don't pretty-print them into a paragraph.
- **Don't redact in the dashboard what the API would return to the same user.** If the user could see it via `curl`, hiding it in the UI just makes the UI less useful than the API — see [`api-first`](api-first.md).
- **Detail views, not modals**, for anything the user might want to keep open alongside another page.

## Quick checklist

For a new surface that represents an event, ask:

1. Does the user see *why* it's in this state, in their own words?
2. Can the user see the raw payload without leaving the page?
3. Can they tell what triggered it (source, caller, broadcast, attempt #)?
4. If they wanted to file a support ticket, would they have everything in one screen to paste in?

If "no" to any of these, the surface is hiding signal.

## Anti-patterns

- A status pill (`Failed`) with no reason and no link to the payload.
- "Something went wrong. Contact support." with no error code, request ID, or timestamp.
- A webhook log that shows pass/fail but not the response body.
- A pretty-printed summary that doesn't expose the original JSON anywhere.
- Redacting fields in the dashboard that the API returns unredacted to the same key.

## Related

- [`table-required-fields`](table-required-fields.md) — debug fields live on the detail page, not in the main table.
- [`api-first`](api-first.md) — the dashboard shouldn't be a worse view of the data than the API.
- [`using-time`](using-time.md) — every event needs a precise timestamp behind the relative label.
