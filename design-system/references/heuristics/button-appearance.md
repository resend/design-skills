# Button appearance

> Guideline, not a rule. Helps pick between Resend's button appearances, sizes, states, and composition options. Live reference: `/design/components/button`.

## The decision

Given a button you need to render, which `appearance` does it take, at which `size`, and how should it be composed (icon? shortcut?)?

## Appearances — intent and hierarchy

Four roles. Pick the one that matches the action's importance on this screen.

| Appearance | Role | Notes |
|------------|------|-------|
| `white` | **Primary** — the most important action on the page | Inverted black/white, flips in dark mode. One per screen. |
| `gray` | **Secondary** — the obvious alternative path | Sits next to the primary. |
| `fade` | **Tertiary / subtle** | Cancel, "Not now", dense action lists, low-stakes escape hatches. |
| `red` / `fade-red` | **Destructive** | `red` for the confirmation moment; `fade-red` for destructive actions in dense surfaces (row context menus, hover toolbars). |

> `fade-gray` also exists in code as a quieter-than-gray secondary. It isn't a documented role on `/design/components/button` — reach for it only when a plain `gray` would visually equal the primary and the design genuinely needs another step quieter. When in doubt, stick to the four roles above.

## Rules of thumb for hierarchy

1. **One primary per screen.** Two `white` buttons force the user to pick a winner the UI should have picked.
2. **Pair the primary with a gray or fade.** Confirm + cancel is `white` + `gray`, or `white` + `fade`. Never `white` + `white`.
3. **Destructive primaries are still primaries.** A `red` button counts as the screen's primary — don't also show a `white` next to it. The escape partner is `fade` or `gray`.
4. **Use `fade-red` in dense surfaces.** Row menus, list-item hover toolbars, and inline destructive actions look hostile in solid red. Save `red` for the confirmation step.
5. **Appearance is a role, not a colour.** "I want a green button" isn't a request the design system answers.

## Sizes

Two sizes adapt to context.

| Size | Use when |
|------|----------|
| `'1'` | **Compact.** Dense interfaces — row toolbars, inline cells, tag-adjacent actions. |
| `'2'` | **Default.** Forms, dialogs, page-level CTAs. Use this unless density forces `'1'`. |

Match the row's other elements (input height, tag size) before reaching for a different size.

> A `'3'` exists in code for oversized hero / marketing-adjacent CTAs. It isn't part of the documented size set — treat it as the exception, not the default.

## States

Two states communicate interactivity. Both use the `state` prop, not separate booleans.

| State | Meaning | How |
|-------|---------|-----|
| `loading` | Action in progress | `state="loading"` — already prevents interaction; do **not** also set `disabled`. |
| `disabled` | Action not available | `state="disabled"` — pair with a tooltip or inline note explaining why (see [`disable-vs-hide`](disable-vs-hide.md)). |

Disabled buttons must be visually inert. No animated icons, no hover effects — see [`affordance`](affordance.md).

## Composition — icons and shortcuts

Buttons can carry an icon and/or a shortcut hint. There are conventions about where each goes.

### Icons

- **Left side → the icon names the action.** `+ Add domain`, `↻ Retry`, `↓ Export`. The icon reinforces the verb.
- **Right side → the icon names the destination.** `Details ›`, `Settings ›`, `Open in new tab ↗`. The icon hints where the click takes the user.

Never put an action icon on the right or a navigation icon on the left — it inverts the reader's expectation.

### Shortcuts

- **For power users.** Surface a keyboard shortcut in a button when the action is frequent enough to deserve muscle memory.
- **Limit to two modifiers.** `⌘ ↵` is fine; `⌘ ⌥ ⇧ K` is not — reserve longer combos for a command palette or settings, not inline button affordances.
- A shortcut hint doesn't replace a tooltip on an IconButton — see [`complementary-information`](complementary-information.md).

## Button vs IconButton

Pick `IconButton` when **all** of these hold:

- The action isn't used often enough on this screen for the icon meaning to feel learned.
- The icon is unambiguous (or a tooltip removes the ambiguity).
- Space is tight enough that a labelled button would crowd the layout.

Pick `Button` when the action is the obvious thing to do here, when the label adds clarity, or when the action is core enough to warrant a full row of attention.

Every `IconButton` needs an `aria-label` *and* a tooltip — without both, the screen-reader user and the hover-less user are guessing.

## Anti-patterns

- Two `white` buttons on the same screen, competing for the click.
- A `red` next to a `white` — the user can't tell which is "the primary".
- Solid `red` for a reversible action like "move to trash" → use `fade-red`.
- `state="loading"` paired with `disabled={true}` — `state` already covers it.
- An action icon on the right (`Export ↓`) or a chevron on the left (`› Details`) — inverts the icon-placement convention.
- A `Button` with three or more keyboard modifiers crammed into the hint.
- `IconButton` with no `aria-label` and no tooltip.
- Using `appearance` as a colour palette ("I need a green button"). Appearance is a role.

## Related

- [`affordance`](affordance.md) — the appearance carries the affordance signal; disabled buttons must look inert.
- [`disable-vs-hide`](disable-vs-hide.md) — when a button should be disabled rather than removed.
- [`complementary-information`](complementary-information.md) — what tooltip text to put on an `IconButton`.
