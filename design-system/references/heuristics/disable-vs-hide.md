# Disable instead of hide

> Guideline, not a rule. Helps the user keep a stable mental map of where features live, even when they can't currently use them.

## The decision

When the current user can't perform an action (wrong plan, wrong role, missing prerequisite), should the control be hidden or visibly disabled?

## Principle

Default to **disabled, with clear feedback explaining why**. The control stays where the user expects to find it — useful if they have access in another account or upgrade later. Hiding silently makes the product feel inconsistent depending on who is logged in.

Switch to **hidden** only when the control reveals data that shouldn't be exposed to this audience at all (billing details to non-admin teammates, account-deletion controls outside an owner context, etc.).

## When to disable

- Plan-gated features (Pro-only buttons shown to free users).
- Role-gated actions (admin-only export shown to members).
- Conditional actions waiting on a prerequisite (e.g. "Send broadcast" disabled until at least one contact is added).
- Anything that is **about** the user's ability to act, not about secret data.

Pair the disabled state with a tooltip, inline note, or upsell that explains the reason. Don't leave the user guessing.

## When to hide

- Sensitive data that has no business reaching this user (billing line items, payment methods, audit log entries scoped to other roles).
- Features that aren't part of this product surface for this audience (admin-only sections inside a member's view).
- Anything where exposing the label itself leaks information.

## Anti-patterns

- A disabled button with no tooltip or message — the user has no idea why they're blocked.
- Hiding an upsell-able feature so users never discover it exists.
- Hiding a button on one screen while keeping it visible on another for the same user — breaks the mental map.
- A disabled button that still animates (loading spinner, hover effects) — see [`affordance`](affordance.md).
