# Design Tokens

Defined in `src/styles/globals.css` using Tailwind CSS v4 with CSS custom properties.

## Color System

Built on Radix UI Colors. Each color has 12 steps + alpha variants.

### Gray Scale

Primary neutral for text, borders, backgrounds:
- `gray-1` to `gray-10` — full opacity
- `gray-a1` to `gray-a10` — semi-transparent alpha
- `light-gray-*` — forced-light context variants

### Semantic Colors

| Color | Usage | Example Classes |
|-------|-------|-----------------|
| `violet` | Brand accent | `bg-violet-3`, `text-violet-11`, `border-violet-4` |
| `green` | Success, active | `bg-green-3`, `text-green-11` |
| `red` | Error, destructive | `bg-red-3`, `text-red-11` |
| `yellow` | Warning | `bg-yellow-3`, `text-yellow-11` |
| `blue` | Info | `bg-blue-3`, `text-blue-11` |
| `orange` | Caution | `bg-orange-3`, `text-orange-11` |
| `sand` | Neutral warm | `bg-sand-3`, `text-sand-11` |
| `cyan` | Secondary accent | `bg-cyan-3`, `text-cyan-11` |

### Color Step Convention

| Step | Usage |
|------|-------|
| 1-2 | Subtle backgrounds |
| 3-4 | UI element backgrounds, borders |
| 5-6 | Hovered/active states |
| 7-8 | Solid backgrounds |
| 9-10 | High-contrast text, solid fills |
| 11-12 | Maximum contrast text |

### Backgrounds

Light: `#fdfdfd` — Dark: `#000`

## Typography

| Token | Font | Usage |
|-------|------|-------|
| `font-sans` | Inter | Body text, UI (default) |
| `font-display` | ABC Favorit | Large headings (size 7-8) |
| `font-domaine` | Domaine | Serif accents |
| `font-mono` | Commit Mono | Code, monospace |

Use `Heading` and `Text` components with `size` prop rather than raw Tailwind text classes.

## Sizing Scale

| Size | Height | Padding | Text | Radius |
|------|--------|---------|------|--------|
| `'1'` | `h-6` (24px) | `px-2` | `text-xs` | `rounded-lg` |
| `'2'` | `h-8` (32px) | `px-3` | `text-sm` | `rounded-xl` |
| `'3'` | `h-10` (40px) | `px-3` | `text-sm` | `rounded-xl` |

### Border Radius

| Class | Value | Usage |
|-------|-------|-------|
| `rounded-lg` | 0.5rem | Size 1 components |
| `rounded-xl` | 0.75rem | Size 2-3 components |
| `rounded-2xl` | 1rem | Banners, larger elements |
| `rounded-3xl` | 1.5rem | Cards |
| `rounded-4xl` | 2rem | Dialogs |

## Shadows

`--shadow-3xl` (large), `--shadow-4xl` (extra-large), `--shadow-button` (button-specific).

## Animations

**Scale & Fade:** `animate-open-scale-in-fade`, `animate-open-scale-up-fade`, `animate-close-scale-out-fade`
**Slide & Fade:** `animate-open-slide-up-fade`, `animate-open-slide-down-fade`, `animate-close-slide-up-fade`, `animate-close-slide-down-fade`
**Utility:** `animate-shine`, `animate-disco`, `animate-scroll-x`, `animate-caret-blink`, `animate-accordion-slide-down/up`, `animate-collapsible-slide-down/up`, `animate-fade-in`, `animate-fade-out`

### Custom Utilities

| Class | Effect |
|-------|--------|
| `fade-in-black` | Horizontal fade mask |
| `bg-shine` | Animated shine effect |
| `bg-gradient-fade` | Gradient fade background (Banner) |
| `effect-font-styling` | Display font styling for headings 7-8 |

## Dark Mode

Via Tailwind `dark:` prefix. Background flips `#fdfdfd` → `#000`. Button `appearance="white"` inverts. Gray scales adjust via Radix. Use `dark:` only for manual overrides.
