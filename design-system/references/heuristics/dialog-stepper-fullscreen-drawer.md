# Dialog, stepper, full-screen page, or drawer

> Guideline, not a rule. Use this to pick a container for a task; escalate to `@design` when more than one option seems reasonable.

## The decision

When a user takes an action, where does it happen? In a dialog over the current page, in a multi-step flow, on a dedicated full-screen page, or in a drawer alongside the current page?

## Three signals to weigh

1. **Workload** — does the task need guidance because of complexity or several decisions?
2. **Occurrence** — is it done many times, or rarely?
3. **Context** — where does the task begin, and where should it end?

The right container minimises the cognitive cost of those three for the user.

## Dialog

For quick actions that may be done more than once. Ideal when the result is immediately visible on the previous page after closing.

- Example: editing a segment name. Fast, low decision count, no extra guidance, result visible behind.
- Reuses the existing context — the user doesn't lose where they were.

**Best practices**
- Avoid a dialog inside another dialog — it creates complex stacking and accessibility issues.
- A dialog can close via the `x` button, clicking the overlay, or the cancel button. **If the user has entered data in an input field, restrict closing to the `x` button** to prevent accidental data loss.

## Stepper

For complex but sporadic tasks that need guidance or multiple decisions. Ideal when the task ends on a different page than it began.

- Example: adding a domain. The user makes a few key decisions; after completion they land on a page that still needs attention.
- Each step is a small win that signals progress on a heavy, one-off task.

## Full-screen page

For tasks that need focus and freedom from surrounding distractions. Ideal for tasks with a temporary state that can be saved or discarded later, such as editors.

- Example: editing the unsubscribe page. The user concentrates on a single task without the outer dashboard chrome.

## Drawer

For long, complementary content that should not disrupt the current task.

- Example: opening an API reference next to the page the user is working on.
- The drawer adds context — it doesn't take over.

## Quick chooser

| Task profile | Container |
|--------------|-----------|
| Fast, repeated, result visible on the same page | Dialog |
| Multi-decision, rare, lands on another page | Stepper |
| Single deep-focus task with save/discard state | Full-screen |
| Auxiliary reference content next to the page | Drawer |

## Anti-patterns

- Dialog inside dialog.
- Dialogs that close on overlay click while the user has typed unsaved input.
- Stepper for a task that fits in one screen and is done frequently.
- Full-screen for a quick edit the user repeats often.
- Drawer used as a primary task surface instead of a complement to the page.
