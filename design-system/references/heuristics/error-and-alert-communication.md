# Error and alert communication

> Guideline, not a rule. Helps choose how visibly an error should appear, based on how predictable it is and how much the user needs to deal with it now.

## The decision

When something goes wrong, where does the message live? Inline next to the input, as an in-page alert, as a global notification, or as a full page error?

## Principle

Match the surface to the predictability and scope of the error. Predictable, local problems get local feedback; unpredictable, page-breaking problems get prominent surfaces. The user should never have to hunt for the explanation of a failure they just caused.

## Form inline error

Use to **prevent predictable mistakes from happening**, or to point at the exact field that's wrong.

- Validation rules the form already knows (required, format, length).
- Inline, next to or below the offending field, via `TextField.Error`.
- Fixes the user's attention on what to change.

## Alert (in-page banner)

Use for server errors, data validation, or heavy tasks whose outcome the form can't predict.

- The action was attempted; the system responded with something the user didn't know in advance.
- Typically lives at the top of the section that owns the action.
- Use the `Banner` primitive with `red` / `yellow` / `green` / `blue` per severity.

## Global notification

Use for any alert that needs the user's immediate attention regardless of which page they're on.

- Account suspended, billing failed, service-wide incidents.
- Persists across navigation until acknowledged or resolved.
- Reserve for rare, high-priority signals — overuse trains users to dismiss them.

## Page error

Use for uncaught runtime errors that block the entire page from rendering.

- The page can't recover on its own; the user must reload, navigate away, or be redirected.
- Should explain in plain language what happened and offer a next step.

## Quick chooser

| Problem profile | Surface |
|-----------------|---------|
| Predictable, scoped to a field | Inline error |
| Unpredictable, scoped to a flow or section | Alert / Banner |
| Account- or product-wide, must not be missed | Global notification |
| Page cannot render at all | Page error |

## Anti-patterns

- Toast notifications for validation errors that should have been inline.
- Inline-only errors for server failures the user can't fix at the input.
- Global notifications for routine, recoverable issues — desensitises the user.
- Page errors that say "Something went wrong" with no recovery path.
