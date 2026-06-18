# Disable instead of hide

> Guideline, not a rule. Helps the user keep a stable mental map of where features live, even when they can't currently use them.

## The decision

When the current user can't perform an action (wrong plan, wrong role, missing prerequisite), should the control be hidden or visibly disabled?

## Principle

Disable when **this user can change their own state to unlock the control** — upgrade a plan, add the missing prerequisite, finish the step before. Hide when they can't, or when the control would leak data they shouldn't see.

A disabled control with no recovery path is just noise: a member who can't promote themselves to admin doesn't benefit from seeing a greyed-out admin button on every page.

## When to disable

- Plan-gated features the user can unlock themselves (Pro-only buttons shown to free users — pair with an upsell or paywall).
- Conditional actions waiting on a prerequisite (e.g. "Send broadcast" disabled until at least one contact is added).
- Anything that is **about** the user's ability to act *right now*, with a recovery path they control.

Pair the disabled state with a tooltip, inline note, or upsell that explains the reason and points at the unlock. Don't leave the user guessing.

## When to hide

- Role-gated actions the user has no way to grant themselves (admin-only export shown to members who can't become admins).
- Sensitive data that has no business reaching this user (billing line items, payment methods, audit log entries scoped to other roles).
- Features that aren't part of this product surface for this audience (admin-only sections inside a member's view).
- Anything where exposing the label itself leaks information.

## Anti-patterns

- A disabled button with no tooltip or message — the user has no idea why they're blocked.
- A disabled control the user can never unlock themselves (admin-only action shown to a member) — hide it.
- Hiding an upsell-able feature so users never discover it exists.
- Hiding a button on one screen while keeping it visible on another for the same user — breaks the mental map.
- A disabled button that still animates (loading spinner, hover effects) — see [`affordance`](affordance.md).
