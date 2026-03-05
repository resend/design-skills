# Component Patterns

## CVA (Class Variance Authority)

All variants use CVA for type-safe styling.

```tsx
import { cva, type VariantProps } from 'class-variance-authority';
import { cn } from '@/lib/cn';

const myVariants = cva('base-classes', {
  variants: {
    appearance: {
      gray: 'bg-gray-a2 border-gray-a3 text-gray-9',
      white: 'bg-black text-white dark:bg-white dark:text-black',
    },
    size: {
      '1': 'h-6 text-xs px-2 rounded-lg',
      '2': 'h-8 text-sm px-3 rounded-xl',
    },
  },
  defaultVariants: { appearance: 'gray', size: '2' },
});

interface MyProps extends React.ComponentProps<'div'>, VariantProps<typeof myVariants> {}

function MyComponent({ appearance, size, className, ...props }: MyProps) {
  return <div className={cn(myVariants({ appearance, size, className }))} {...props} />;
}
```

### Conventions

- String literal sizes: `'1'`, `'2'`, `'3'` — never numbers
- `appearance` = visual style, `size` = dimensions, `state` = interactive state
- Always pass `className` through CVA for merge support
- Use `cn()` from `@/lib/cn` (wraps `tailwind-merge` + `clsx`)

## Compound Component Pattern

### Object namespace (TextField, Avatar, BulkActions)

```tsx
export const TextField = { Root, Slot, Input, Error };

<TextField.Root>
  <TextField.Slot>...</TextField.Slot>
  <TextField.Input size="2" />
</TextField.Root>
```

### Named exports (Select, Dialog, Tooltip, DropdownMenu)

```tsx
import * as Select from '@/ui/select';
<Select.Root>
  <Select.Trigger />
  <Select.Content><Select.Item value="x">X</Select.Item></Select.Content>
</Select.Root>
```

Object namespace for tightly coupled parts. Named exports for Radix-based primitives.

## Slot Pattern (asChild)

Uses `@radix-ui/react-slot` to merge props onto a child element:

```tsx
<Button asChild><Link href="/settings">Settings</Link></Button>
<Dialog.Trigger asChild><IconButton appearance="fade"><IconSettings /></IconButton></Dialog.Trigger>
```

## TextField Slot System

Auto-adjusts input padding via ResizeObserver:

```tsx
<TextField.Root>
  <TextField.Slot><IconSearch /></TextField.Slot>
  <TextField.Input placeholder="Search..." />
  <TextField.Slot><Button size="1" appearance="fade">Clear</Button></TextField.Slot>
</TextField.Root>
```

Max one slot before Input, one after. Only `TextField.Slot` and `TextField.Input` as direct children.

## State Management

Use `state` prop, not individual booleans:

```tsx
<Button state="loading">Save</Button>
<TextField.Input state="disabled" />
<TextField.Input state="read-only" />
```

## Shared Styling

`src/ui/shared.ts` exports constants for dropdown/floating bar consistency:

```tsx
import { dropdown, floatingBottomBar } from '@/ui/shared';
<div className={cn(dropdown.content.appearance, dropdown.content.sizing)}>
  <div className={cn(dropdown.item.sizing, dropdown.item.appearance.gray)}>Item</div>
</div>
```

## Server vs Client Components

Default to Server Components. `'use client'` only at lowest interactive leaf.

**Already client:** TextField, Checkbox, Dialog, Drawer, Collapsible, Calendar, BulkActions.
**Server-safe:** Button, Heading, Text, Tag, Banner, Card, EmptyState, Kbd.

## Accessibility

- Always `aria-label` on IconButton
- Dialog auto-manages focus trap + escape
- Banner uses `role="alert"`
- `TextField.Error` wires `aria-describedby` automatically
- Checkbox supports `'indeterminate'`
