# Audit Rubric

Seven categories to check. Categories 1–5 and 7 flag violations against existing rules. Category 6 flags gaps in the rules themselves.

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

**Scope:** `src/app/(dashboard)*/**/*.tsx` and `src/components/**/*.tsx`

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

**Hand-rolled dropdown menus — `use-dropdown-tokens`**

Files outside `src/ui/` that compose their own dropdown / menu surface instead of reusing `DropdownMenu` from `@/ui/dropdown-menu` or the shared `dropdown.*` token groups in `src/ui/shared.ts`.

Detect when a file matches **both** of these signals:
1. Imports or renders `Popover.Content` (from `@/ui/popover`) — or a floating `div` with `max-h-[` and `overflow-y-auto` — as the menu surface.
2. Contains child item elements (`<button>`, `<a>`, or role="menuitem") whose `className` re-creates dropdown item styles: a combination of `rounded-(lg|xl)`, `px-2`, and any of `bg-gray-a2`, `text-gray-9`, `text-gray-10`, `text-gray-a10`, or selected-state pairs like `bg-gray-a2 text-gray-10`.

**Severity:** `warn`
**Suggestion:** Compose with `DropdownMenu.Root/Content/Item` from `@/ui/dropdown-menu`. If a custom surface is genuinely required (e.g. a typeahead anchored to an editor), reuse the `dropdown.content.appearance`, `dropdown.item.sizing`, and `dropdown.item.appearance.gray` token groups exported from `@/ui/shared` instead of redefining the classes inline.
**Design ref:** `/design/components/dropdown-menu`

**Severity:** `warn`
**Design ref:** `/design/components/<component-name>`

---

## Category 3 — Token misuse

**Scope:** `src/app/(dashboard)*/**/*.tsx` and `src/components/**/*.tsx`

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

---

## Category 7 — Copy & brand voice

**Scope:** `src/app/(dashboard)*/**/*.tsx`

**What to check:** User-facing strings — JSX text nodes, `placeholder`, `aria-label`, `title` prop values, and string arguments to `description`, `label`, and similar props.

**How to run:** Two passes.

1. **Grep pass** — fast, deterministic checks (rules 7a, 7c, 7d, 7e partial)
2. **LLM pass** — glob `src/app/(dashboard)/*/page.tsx` plus key nested routes to get ~15 representative pages. Read each file, extract hardcoded strings with their line numbers, then apply the remaining rules (7b, 7d partial, 7e).

Each violation finding requires `file`, `line`, `category: "copy"`, `severity`, `rule_id`, `suggestion`, and `design_ref`.

---

### 7a — Sentence case

**Rule ID:** `use-sentence-case` · **Severity:** `warn`

Resend uses sentence case everywhere: headings, buttons, labels, nav items, error messages. Title Case is a violation.

**Grep** for common patterns where 2+ consecutive words are title-cased inside JSX text nodes (not proper nouns):
- `>Save Changes`, `>Get Started`, `>Learn More`, `>View All`, `>Add New`, `>Create New`, `>Sign In`, `>Sign Up`, `>Log Out`, `>Read More`, `>Try Again`

**LLM pass:** Flag any button label, heading, or CTA with 2+ title-cased words that are not proper nouns (product names, company names, acronyms).

**Suggestion:** Use sentence case — e.g., "Save Changes" → "Save changes", "Get Started" → "Get started"
**Design ref:** `/design` (brand guidelines — typography rules)

---

### 7b — Typos and misleading copy

**Rule ID:** `copy-typo` · **Severity:** `error`

**Grep** for known common misspellings inside string literals and JSX text:
- `recieve` → `receive`
- `occured` / `occurance` → `occurred` / `occurrence`
- `seperate` → `separate`
- `existance` → `existence`
- `privelege` / `priviledge` → `privilege`
- `definitly` → `definitely`
- `sucessful` / `succesful` → `successful`

**LLM pass:** For each sampled file, flag any clear spelling errors, double spaces, grammatically broken phrases, or **misleading copy** — text that describes a feature, action, or state incorrectly (e.g. a button labelled "Delete" that actually archives, or an error message that names the wrong field).

**Suggestion:** Fix the specific typo, grammar issue, or inaccurate description.

---

### 7c — Vague CTAs

**Rule ID:** `avoid-vague-cta` · **Severity:** `info`

CTAs should be specific and action-oriented. Generic text fails clarity and accessibility.

**Grep** for these patterns as complete (or near-complete) JSX text node content:
- `>Click here<` / `>Click Here<`
- `>here<` / `>Here<` as standalone link text
- `>Read more<` / `>Read More<` as a standalone CTA

**Suggestion:** Replace with a specific action — "Click here" → "View API keys", "Read more" → "Read about webhooks"

---

### 7d — Brand terminology

**Rule ID:** `brand-terminology` · **Severity:** `warn`

Key terms must be spelled and capitalised consistently.

**Grep** for these patterns in JSX text nodes and string literals:

| Pattern | Correct | Note |
|---------|---------|------|
| `e-mail` / `E-mail` | `email` / `Email` | No hyphen |
| `RESEND` in UI text | `Resend` | Product name is not all-caps |
| `Re-send` | `Resend` | No hyphen in product name |

**LLM pass:** Review sampled strings for inconsistent use of feature names and product terms (e.g. "API Key" vs "API key", "web hook" vs "webhook").

**Suggestion:** Use the correct form: `email`, `Resend`, `webhook`, `API key`.
**Design ref:** `/design` (brand guidelines)

---

### 7e — Tone and voice

**Rule ID:** `brand-voice` · **Severity:** `info`

Resend's voice is direct, minimal, and developer-focused. Flag copy that drifts into marketing superlatives, excessive enthusiasm, or unnecessary filler.

**Grep** for these patterns inside JSX text nodes:
- Marketing superlatives: `powerful`, `amazing`, `supercharge`, `seamlessly`, `effortlessly`, `incredible`, `world-class`
- Exclamation overuse: a `!` at the end of a text node in error messages, form labels, or confirmations (celebratory empty-state messages are fine)

**LLM pass:** For sampled files, flag strings that feel inconsistent with the Resend brand voice — too promotional, too casual, or padded with filler phrases ("just", "simply", "easily").

**Suggestion:** Rewrite to be direct and specific. "Powerful email delivery" → "Email delivery for developers". Remove filler adverbs.
**Design ref:** `/design` (brand guidelines — design principles)
