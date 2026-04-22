# Linear Delivery Playbook

One ticket per audit run. The ticket is a curated summary, not a dump of every finding.

## Tools

- `linear:list_issues` — check for existing open or cancelled issues
- `linear:create_issue` — open a new issue
- `linear:create_comment` — add a weekly update to an existing open issue

## Step 1 — Check for existing issues

Call:

```
linear:list_issues
  filter: { label: "design-audit" }
  team: "Design"
```

Evaluate the results:

- **An open issue exists** (state: unstarted, started, or in-progress) → go to Step 3 (comment). Do not create a duplicate.
- **A cancelled issue exists** → go to Step 1a before deciding whether to create.
- **No issue exists** → go to Step 2 (create).

## Step 1a — Compare against cancelled issue

Read the body of the most recently cancelled `design-audit` issue. Extract the `rule_id` values mentioned in its findings.

Compare them against the `rule_id` values in the current run's top findings:

- **All top findings are already in the cancelled issue** (same rule_ids, same or similar files) → the team already reviewed and dismissed these. Do not create a new issue. Print a note to the session transcript: "Skipped — all findings match cancelled issue {ID}." Stop.
- **At least one top finding has a rule_id not present in the cancelled issue** → there is something new. Proceed to Step 2, but **exclude from the ticket body** any findings that were already present in the cancelled issue. Only surface the new findings.

## Step 2 — Create a new issue

```
linear:create_issue
  title: "Design audit — {YYYY-MM-DD}"
  team: "Design"
  project: "Design Audit"  # https://linear.app/resend/project/design-audit-59b6c51f2dee
  labels: ["design-audit"]
  state: "Triage"
  priority: {derived from report — see Priority rules below}
  body: <rendered markdown from report-format.md>
```

### Priority rules

Pick the highest priority that applies:

| Condition | Priority |
|-----------|----------|
| Any `error` severity finding | Urgent |
| `warnings` > 10 or a `rubric_candidate` present | High |
| Only `warn` findings, ≤ 10 | Medium |
| Only `info` findings | Low |

## Step 3 — Comment on an existing open issue

```
linear:create_comment
  issue_id: <id of the open issue>
  body: "## Update — {YYYY-MM-DD}\n\n<rendered markdown from report-format.md>"
```

## Failure handling

If any Linear MCP call fails:
1. Print the full error and the complete rendered markdown report to the session transcript.
2. Do not retry more than once.
3. Do not silently continue — the transcript is the fallback record.
