# Tables should include only required fields

> Guideline, not a rule. Keeps list views scannable and avoids visual clutter from columns that are mostly empty.

## Principle

The main table view shows the columns that almost every row will have a value for. Optional fields with frequent gaps belong on the detail page, not the list.

## How to apply this

1. List every column the table could show.
2. For each, ask: *what fraction of rows will have a meaningful value here?* If a large share will be empty, move it to the detail view.
3. Keep identifiers, status, and the field the user filters or sorts on most. Push descriptive or optional metadata to the row's detail page.

## Examples

- A contacts table with `Email` populated everywhere and `Name` filled in for a small minority → keep `Email`, move `Name` to the detail view (or merge it under the email).
- A domains table where `Region` is always set but `Custom return path` is rare → `Region` stays, `Custom return path` moves.

## Anti-patterns

- Adding columns because they exist on the resource, not because users need them at a glance.
- A table whose main column is half blank — it reads as data missing, not as "this field is optional".
- Burying the column the user actually filters by behind a long row of optional metadata.

## Related

- [`expose-debuggable-data`](expose-debuggable-data.md) — the detail view is also where raw / debug fields belong.
- [`friendly-names-over-ids`](friendly-names-over-ids.md) — when collapsing columns, prefer the friendly name over an ID.
