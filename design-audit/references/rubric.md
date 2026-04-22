# Audit Rubric

Six categories to check. Categories 1–5 flag violations against existing rules. Category 6 flags gaps in the rules themselves.

Each violation finding requires: `file`, `line`, `category`, `severity`, `rule_id`, `suggestion`, `design_ref`.

## Setup

Before running any category, read `sidebar-data.ts` to extract the **alias map** and **ignore list**. The alias map normalises component names (e.g. `"command"` → `"combobox"`, `"avatar team"` → `"avatar"`). Apply it when matching `src/ui/` filenames against `documented-components.json`.

---

## Category 1 — Missing documentation

**Scope:** `src/ui/` top-level `.tsx` files (not subdirectories like `icons/`)

**How to check:**
1. Glob `src/ui/*.tsx` to get the list of component files
2. Strip the `.tsx` extension and convert to lowercase to get a component name
3. Apply the alias map from `sidebar-data.ts` if applicable
4. Skip any name that appears in the ignore list (`_common`, `layout`, `shared`, `page`, `todo`)
5. Check whether the resolved name appears in `documented-components.json`
6. Any component file NOT in `documented-components.json` is a finding

**Severity:** `warn`
**Rule ID:** `missing-docs`
**Design ref:** `/design` (component documentation guide)
**Suggestion:** "Add a documentation page at `src/app/(internal)/design/components/<name>/page.tsx` following the component-documentation-guide.md template."

---

## Category 2 — Component substitution

**Scope:** `src/app/(dashboard)*/**/*.tsx`

**What to grep for:**

| Pattern | Rule ID | Suggestion |
|---------|---------|------------|
| `<button` (HTML element, not component) | `use-button-primitive` | Use `<Button>` from `@/ui/button` |
| `<input` | `use-text-field-primitive` | Use `TextField.Input` from `@/ui/text-field/text-field` |
| `<select` | `use-select-primitive` | Use `Select.Root/Trigger/Content/Item` from `@/ui/select` |
| `<dialog` | `use-dialog-primitive` | Use `Dialog.Root/Content` from `@/ui/dialog` |
| `<textarea` | `use-text-area-primitive` | Use `TextField.Input` with `asChild` or a dedicated textarea primitive |

Also flag files that appear to re-implement primitives already in `src/ui/`:
- Any file outside `src/ui/` that defines a component named `Switch`, `Checkbox`, `TextField`, `TextInput`, `Modal`, `Tooltip`, or `Drawer` (case-insensitive match on the export name)

**Severity:** `warn`
**Design ref:** `/design/components/<component-name>`

---

## Category 3 — Token misuse

**Scope:** `src/app/(dashboard)*/**/*.tsx`

**What to grep for:**

| Pattern | Description | Rule ID | Severity |
|---------|-------------|---------|----------|
| `w-\[[0-9]` | Arbitrary width (e.g. `w-[13px]`) | `use-sizing-scale` | `info` |
| `h-\[[0-9]` | Arbitrary height | `use-sizing-scale` | `info` |
| `text-\[[0-9]` | Arbitrary font size (e.g. `text-[14px]`) | `use-text-scale` | `warn` |
| `bg-\[#` | Hex color background | `use-color-token` | `warn` |
| `text-\[#` | Hex color text | `use-color-token` | `warn` |
| `border-\[#` | Hex color border | `use-color-token` | `warn` |

**Suggestion for color violations:** Replace hex literals with semantic color tokens from `design-system/references/design-tokens.md` (e.g. `bg-gray-2`, `text-gray-11`).
**Suggestion for sizing violations:** Use the sizing scale defined in `design-system/SKILL.md` or a standard Tailwind step.
**Design ref:** `/design` (design tokens section)

---

## Category 4 — Deprecated component usage

**Scope:** `src/app/(dashboard)*/**/*.tsx`

**How to check:**
1. For each component in `documented-components.json`, read its design page at `src/app/(internal)/design/components/<name>/page.tsx`
2. Search that file for a `Deprecated` section header or a `deprecated` prop example
3. If found, note the component name as deprecated
4. Grep `src/app/(dashboard)*` for `from '@/ui/<deprecated-component-name>'` imports
5. Each importer is a finding

**Severity:** `warn`
**Rule ID:** `deprecated-component`
**Suggestion:** "Check the `/design/components/<name>` page for the recommended replacement."
**Design ref:** `/design/components/<name>`

---

## Category 5 — Pattern candidates

**Scope:** `src/app/(dashboard)*/**/*.tsx`

**How to identify:**
1. Read `documented-patterns.json` (currently `[]` — no patterns documented yet)
2. Look for structural compositions that repeat across ≥ 3 dashboard files, such as:
   - Empty state layouts (icon + heading + description + action button)
   - Filter bars (search input + dropdown filters + tag list)
   - Confirm dialogs with title + description + cancel/confirm buttons
   - Table toolbar patterns (bulk action bar + count + primary action)
   - Settings sections (heading + description + control on the right)
3. A candidate must NOT already exist in `documented-patterns.json`

**Output:** Emit as `pattern_candidates` in the report, not as `violations`. These are proposed improvements, not errors.

**Fields required:**
- `proposed_name` — a slug name for the pattern (e.g. `empty-state`, `confirm-dialog`)
- `occurrences` — array of `file:line` where the pattern appears
- `rationale` — one sentence on why this should be a documented pattern

---

## Category 6 — Rubric candidates

**Scope:** Everything observed during the full audit run.

**What to look for:**

A rubric candidate is a recurring misuse or anti-pattern that appears in ≥ 3 dashboard files but is **not covered by any existing rule** in this rubric. Examples:
- A `Tooltip` always composed without an `aria-label` on its trigger
- A layout idiom (e.g. a card header with icon + title + action) that bypasses a primitive but also doesn't match any existing substitution rule
- A CSS utility or custom class recreating something the DS already provides, but not caught by the token-misuse grep patterns
- Consistent `className` overrides on a primitive that suggest a missing variant

**How to identify:**
1. While running categories 1–5, note anything that feels like a systemic problem but doesn't fit an existing rule
2. Look for it in at least 3 files before flagging — one-offs are noise
3. Confirm it's not already covered by an existing rule ID

**Output:** Emit as `rubric_candidates` in the report, not as `violations`. These are proposed additions to the design skill or this rubric.

**Fields required:**
- `proposed_rule_id` — a slug for the new rule (e.g. `tooltip-accessible-trigger`, `card-header-primitive`)
- `occurrences` — array of `file:line` showing the pattern
- `rationale` — one sentence on why this should be a rule
- `suggested_fix` — what the rule should prescribe (a primitive to use, a prop to set, a pattern to follow)
