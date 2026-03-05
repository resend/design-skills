---
name: resend-design-skills
description: Use when needing Resend design resources. Routes to brand guidelines, visual identity, UI components, design tokens, and marketing page patterns.
metadata:
    author: resend
    version: "1.0.0"
---

# Design Skills

A collection of design-related skills for Claude Code.

## Available Skills

| Skill | Description | Path |
|-------|-------------|------|
| `resend-brand` | Applies Resend's brand colors, typography, and visual identity to marketing materials and artifacts | [brand-guidelines/SKILL.md](brand-guidelines/SKILL.md) |
| `resend-design-system` | UI component APIs, design tokens, and composition patterns for building product UI in the Resend codebase | [design-system/SKILL.md](design-system/SKILL.md) |
| `marketing-pages` | Page structure, component reuse rules, and public primitives for creating/editing marketing pages in `src/app/(website)/` | [marketing-pages/SKILL.md](marketing-pages/SKILL.md) |

## When to use which skill

- **`resend-brand`** — Marketing pages, social graphics, presentations, documents, and any external-facing visual content
- **`resend-design-system`** — Product UI, `src/ui/` components, design tokens, and code patterns inside the Resend codebase
- **`marketing-pages`** — Creating, updating, or deleting marketing pages in `src/app/(website)/`, including page structure, public primitives, and SEO metadata

## Structure

Each skill lives in its own folder with a `SKILL.md` file that defines:
- Frontmatter with `name` and `description`
- Guidelines, rules, and reference material

```
design-skills/
├── SKILL.md              # This index file
├── brand-guidelines/
│   └── SKILL.md          # Resend brand skill
├── design-system/
│   ├── SKILL.md          # Resend design system skill
│   └── references/
│       ├── components.md
│       ├── design-tokens.md
│       └── patterns.md
├── marketing-pages/
│   ├── SKILL.md          # Marketing pages skill
│   └── references/
│       └── components.md
└── tests/
    └── TESTS.md          # Combined test suite
```
