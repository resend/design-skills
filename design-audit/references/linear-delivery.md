# Linear Delivery Playbook

One ticket per distinct finding or suggestion. Each ticket covers one rule violation type, one missing doc, one pattern candidate, or one rubric candidate — and lists every affected file in its body.

## Tools

- `linear:list_issues` — check for existing or cancelled issues before creating
- `linear:create_issue` — open a ticket for a finding
- `linear:create_comment` — add context to an existing open ticket

## What produces a ticket

| Source | Ticket title shape | One ticket per |
|--------|--------------------|----------------|
| `missing_docs` | "Document the `{component}` component" | Component |
| `violations` | "Replace {raw element} with `{primitive}`" / "Fix token misuse in {area}" | rule_id |
| `pattern_candidates` | "Document the `{proposed_name}` pattern" | Proposed pattern |
| `rubric_candidates` | "Add `{proposed_rule_id}` rule to design skill" | Proposed rule |

Violations with the same `rule_id` across multiple files → **one ticket** listing all affected files. Do not create a ticket per file.

## For each ticket to create

### Step 1 — Check for a duplicate or cancelled ticket

Before creating, call:

```
linear:list_issues
  filter: { label: "design-audit" }
  team: "Design"
```

- **An open ticket with the same title or same rule_id already exists** → comment on it with any new affected files. Do not create a duplicate.
- **A cancelled ticket for this same finding exists** → skip it entirely. The team already decided not to act on it. Move to the next finding.
- **No matching ticket** → create it (Step 2).

### Step 2 — Create the ticket

```
linear:create_issue
  title: {title from table above}
  team: "Design"
  project: "Design Audit"  # https://linear.app/resend/project/design-audit-59b6c51f2dee
  labels: ["design-audit"]
  state: "Triage"
  priority: {see Priority rules}
  body: {see Body format}
```

### Priority rules

| Condition | Priority |
|-----------|----------|
| `error` severity | Urgent |
| `warn` severity | Medium |
| `info` severity, pattern candidate, rubric candidate | Low |

### Body format

Plain markdown only. No HTML tags.

```markdown
{one sentence describing the problem}

**Rule:** `{rule_id}` · [Design reference]({design_ref})

**Affected files:**
- `{file}:{line}` — {suggestion}
- `{file}:{line}` — {suggestion}

**Suggestion:** {suggestion}
```

For pattern and rubric candidates:

```markdown
{rationale}

**Occurrences:**
- `{file}:{line}`
- `{file}:{line}`

**Suggested fix:** {suggested_fix}
```

## Failure handling

If any Linear MCP call fails:
1. Print the full error to the session transcript.
2. Print the list of tickets that were not created.
3. Do not retry more than once.
