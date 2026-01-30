# Resend Design Skills

An agent skill that provides Resend's brand guidelines and design system directly in your workflow.

## Installation

```bash
npx skills add resend/design-skills
```

## What's Included

This skill gives Claude access to Resend's complete brand specifications:

### Colors

**Core Palette**
- Resend Black: `#000000`
- Resend White: `#FDFDF`

**Semantic Colors** for communicating state:
- Gray, Red, Amber, Green, Blue with background/foreground pairs

### Typography

- **Domaine Display Narrow** — Display headlines
- **Favorit** — Headings & titles
- **Inter** — Body text
- **CommitMono** — Code

Includes complete type scale with sizes, weights, and line heights.

### Logo Assets

CDN links to official wordmarks and lettermarks in SVG/PNG formats, plus usage restrictions and spacing requirements.

### Design Elements

- Gradients (font, smooth, border, rainbow)
- Glass blur effect
- Noise texture
- Layout patterns (Right Object Scene, Interface Scene, Text Only variants, Big Number)

## Usage

Once installed, Claude will automatically apply Resend's brand guidelines when you:

- Ask for help building UI components
- Request color values or typography specs
- Need logo assets or placement guidance
- Want to ensure designs follow brand principles

### Example Prompts

```
What's the Resend color for error states?
```

```
Help me style this button to match Resend's brand
```

```
What font should I use for headings?
```

## License

MIT
