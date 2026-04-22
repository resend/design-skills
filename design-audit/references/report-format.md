# Report Format

## Curation rules — read before building the report

**Do not list every finding.** The goal is an actionable ticket, not an exhaustive log.

1. **Top findings:** Pick the 5 most impactful findings across all categories. Rank by: severity first (error > warn > info), then by number of affected files. These are the only findings that appear in the Linear ticket body.
2. **Counts only for the rest:** Everything outside the top 5 is summarised as a count (e.g. "12 more warnings across token misuse and substitution").
3. **Skip info-only runs:** If all findings are `info` severity and there are fewer than 5, note "No significant violations found this week" and skip the findings section entirely.
4. **No HTML:** Do not use `<details>`, `<summary>`, or any HTML tags. Linear renders plain markdown only.

---

## JSON structure

Emit this JSON internally (not in the ticket body — use it to build the markdown):

```json
{
  "generated_at": "2026-04-20T09:00:00Z",
  "commit_sha": "<read from git rev-parse HEAD>",
  "summary": {
    "errors": 0,
    "warnings": 0,
    "info": 0,
    "coverage_pct": 0
  },
  "missing_docs": [
    { "ui_file": "src/ui/banner.tsx", "component": "banner" }
  ],
  "violations": [
    {
      "file": "src/app/(dashboard)/settings/page.tsx",
      "line": 42,
      "category": "substitution",
      "severity": "warn",
      "rule_id": "use-button-primitive",
      "suggestion": "Use <Button> from @/ui/button",
      "design_ref": "/design/components/button"
    }
  ],
  "pattern_candidates": [
    {
      "proposed_name": "empty-state",
      "occurrences": ["src/app/(dashboard)/emails/page.tsx:88", "src/app/(dashboard)/domains/page.tsx:34"],
      "rationale": "Icon + heading + description + CTA repeated across 3+ dashboard pages without a documented pattern."
    }
  ],
  "rubric_candidates": [
    {
      "proposed_rule_id": "tooltip-accessible-trigger",
      "occurrences": ["src/app/(dashboard)/settings/page.tsx:14", "src/app/(dashboard)/domains/page.tsx:62"],
      "rationale": "Tooltip trigger consistently lacks aria-label, but no rule currently enforces this.",
      "suggested_fix": "Require aria-label on all Tooltip.Trigger elements that wrap icon-only buttons."
    }
  ]
}
```

`coverage_pct` = `(documented_count / total_ui_components) * 100`, rounded to one decimal.

---

## Markdown template

Use this structure for the Linear ticket body. Fill in only the sections that have findings. Omit empty sections entirely.

```markdown
**{YYYY-MM-DD} · `{SHA_SHORT}` · Coverage: {COVERAGE_PCT}% ({DOCUMENTED}/{TOTAL})**

{ERRORS} errors · {WARNINGS} warnings · {INFO} info

---

### Top findings

{FOR EACH OF THE TOP 5 FINDINGS, one bullet per finding}
- **[{rule_id}]** `{file}:{line}` — {suggestion} → [{design_ref}]({design_ref})

{IF more findings beyond top 5}
_{N} more findings ({REMAINING_ERRORS} errors, {REMAINING_WARNINGS} warnings, {REMAINING_INFO} info) — run the audit locally to see the full list._

---

### Missing documentation ({COUNT} components)

{LIST component names, one per line, max 10. If more: "and N others."}
- `{component}` — `{ui_file}`

---

### Pattern candidates

{FOR EACH candidate}
- **`{proposed_name}`** — {rationale} ({occurrences count} occurrences)

---

### Rubric candidates

{FOR EACH candidate}
- **`{proposed_rule_id}`** — {rationale} Suggested fix: {suggested_fix}

---

_Design audit skill · [Design system](/design)_
```

**Rendering rules:**
- Use plain markdown lists and headers only — no HTML
- `file:line` references as inline code, not links (Linear doesn't resolve repo links)
- Keep the whole body under ~40 lines so it's readable without scrolling
