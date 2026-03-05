# Resend Design Skills

An agent skill collection that provides Resend's brand guidelines and design system directly in your workflow.

## Installation

```bash
npx skills add resend/design-skills
```

## What's Included

### resend-brand

Brand guidelines for marketing materials, social graphics, presentations, and external-facing visual content.

**Colors**
- Resend Black: `#000000` / Resend White: `#FDFDFD`
- Semantic colors for state: Gray, Red, Amber, Green, Blue with background/foreground pairs

**Typography**
- **Domaine Display Narrow** — Display headlines (never in product UI)
- **Favorit** — Headings & titles
- **Inter** — Body text
- **CommitMono** — Code

**Logo Assets**
- CDN links to official wordmarks and lettermarks (SVG/PNG)
- Usage restrictions and clearspace requirements

**Design Elements**
- Gradients (font, smooth, border, rainbow)
- Glass blur effect, noise texture
- Layout patterns (Right Object Scene, Interface Scene, Text Only variants, Big Number)

---

### resend-design-system

Component APIs, design tokens, and composition patterns for building product UI inside the Resend codebase.

**UI Components** — 57+ primitives in `src/ui/` built on Radix UI and styled with CVA:
- **Actions** — Button, IconButton, CopyButton
- **Form** — TextField (compound), Select, Checkbox, Switch, Calendar
- **Display** — Heading, Text, Tag, Banner, Avatar, Card, EmptyState
- **Overlay** — Dialog, Drawer, Popover, Tooltip, ContextMenu, DropdownMenu
- **Navigation** — Tabs, Pagination, Breadcrumb, Link, InternalLink
- **Feedback** — Toast, Skeleton, LoadingDots
- **Icons** — 100+ SVG icons in `src/ui/icons/`

**Design Tokens** from `src/styles/globals.css`:
- Radix-based semantic color palette with 12-step scales
- Typography scale (Inter, ABC Favorit, Domaine, Commit Mono)
- Component sizing scale (1/2/3), border radius, shadows, 20+ animations

**Component Patterns** — CVA conventions, compound components, slot system, Server vs Client boundaries

---

## Usage

Once installed, Claude will automatically apply the right skill based on context:

- Ask for brand colors, typography specs, or logo assets → `resend-brand`
- Build UI components, forms, or pages in the Resend codebase → `resend-design-system`

### Example Prompts

```
What's the Resend color for error states?
```

```
Build a settings form with email validation using Resend's TextField
```

```
Create a confirmation dialog with a destructive delete action
```

```
I'm designing a Resend social graphic. What layout pattern should I use?
```

## License

MIT
