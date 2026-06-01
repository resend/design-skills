# Tag colors are for status, not decoration

> Guideline, not a rule. Reserve `Tag` (and `Badge`-style) colours for communicating state. Decorative colour breaks the visual grammar of the dashboard.

## The decision

You're labelling something on the page — a row state, a category, a property. Which `Tag` appearance does it take, and is colour even the right signal?

## Principle

The Resend dashboard treats colour as a **semantic signal**, not a palette to draw from. When a user sees a coloured `Tag`, they should be able to read it as *the state of something* — and the colour should already tell them how much attention it needs before they read the label.

Three visibility tiers map to three attention levels:

| Tier | Tag colours | What it means | Examples |
|------|-------------|---------------|----------|
| **Neutrals** | `gray`, `sand` | Informational, low priority — the system is doing its job, no user attention required | `Scheduled`, `Queued`, `Sent`, `Cancelled`, `Delivery delayed` |
| **Low–Med visibility** | `blue`, `violet`, `green` | Informational, worth noticing — a meaningful event happened | `Opened` (blue), `Clicked` (violet), `Delivered` (green) |
| **High visibility** | `red`, `yellow`, `orange` | Needs action — something failed, requires a decision, or is at risk | `Bounced` (red), `Complained` (yellow) |

The user should learn this grammar once and have it work everywhere.

## How to apply this

1. **Is the thing you're labelling a status?** If no — it's a tab name, a category, a label the user picked, a count — don't reach for a coloured Tag. Use a `gray` Tag (or no Tag at all).
2. **If yes, which tier does the status sit in?**
   - The user *doesn't* need to do anything → neutral (`gray` / `sand`).
   - Something happened the user might want to know → low–med (`blue` / `violet` / `green`).
   - The user *should* look at this → high visibility (`red` / `yellow` / `orange`).
3. **Within a tier, the specific colour is a stable mapping per status type.** Once `Delivered` is `green`, every place in the product that shows `Delivered` is `green`. Don't re-cast a status into a different colour on a different page.

## When colour isn't the right signal

- **Categories the user creates themselves** (audience names, segment labels, custom topics) — use `gray`. The user gives them meaning; the system shouldn't paint them.
- **Counts / metadata** (`12 contacts`, `3 keys`) — not a status. Use `gray` if a Tag is needed at all, or plain text.
- **Filters and tabs** — let the active state, not colour, communicate selection.

## Anti-patterns

- A `green` Tag on a custom audience name "to make it stand out." Green now stops meaning *Delivered*.
- A `red` Tag on a high-priority broadcast that isn't actually in a failure state. Red is reserved for *needs action*.
- `Blue` for "premium feature" or "new". That's decoration; use a `gray` Tag, a label, or no Tag.
- Two different colours on the dashboard for the same underlying status (e.g. `Delivered` shown as `green` on one page and `blue` on another).
- Using `violet` / `orange` / `sand` as decorative accents on cards or marketing-adjacent surfaces — they belong to the status grammar.
- Reaching for `Tag` where plain text or a single icon would do. Every Tag is a small claim on the user's attention; spending that attention on decoration trains the user to ignore them.

## Related

- [`affordance`](affordance.md) — a status colour is itself an affordance; using it decoratively breaks the contract.
- [`expose-debuggable-data`](expose-debuggable-data.md) — the status colour is the summary; the raw reason and payload live behind the row.
- `Banner` uses the same intent system at the section level (`red` = error, `yellow` = warning, `green` = success, `blue` = info) — see `design-system/SKILL.md`.
