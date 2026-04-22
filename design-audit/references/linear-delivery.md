# Linear Delivery Playbook

How to file or update the weekly design audit issue via Linear MCP.

## Tools

- `linear:list_issues` — check for an existing open audit issue
- `linear:create_issue` — open a new issue
- `linear:create_comment` — add a comment to an existing issue

## Step 1 — Check for an existing open issue

Before creating anything, call:

```
linear:list_issues
  filter: { label: "design-audit", state: { type: "unstarted" } }
  team: "Design"
```

- If **no open issue** is found → proceed to Step 2 (create).
- If **an open issue** is found → proceed to Step 3 (comment).

This prevents duplicate issues when the routine runs more than once before the previous issue is closed.

## Step 2 — Create a new issue

```
linear:create_issue
  title: "Design audit — {YYYY-MM-DD}"
  team: "Design"
  labels: ["design-audit"]
  body: <rendered markdown from report-format.md>
```

The body must be the full markdown report from `report-format.md`. Do not truncate.

## Step 3 — Comment on an existing issue

```
linear:create_comment
  issue_id: <id of the open issue>
  body: "## Update — {YYYY-MM-DD}\n\n<rendered markdown from report-format.md>"
```

Prefix the body with `## Update — {date}` so the thread is scannable.

## Failure handling

If the Linear MCP call fails (tool unavailable, auth error, network error):
1. Print the full error to the session transcript.
2. Print the complete rendered markdown report to the session transcript so findings are not lost.
3. Do not silently continue. The session transcript is the fallback record.

Do not retry more than once. If the second attempt also fails, stop and surface the error.

## Team and label configuration

- **Team:** `Design` (the team name as configured in your Linear workspace)
- **Label:** `design-audit` — create this label in Linear before the first run if it does not exist
- These values are set once when connecting the Linear MCP server and do not need to change per run
