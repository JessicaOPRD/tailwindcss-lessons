---
description: "Use when writing or reviewing Tailwind CSS utility classes, @layer rules, or CSS custom properties in this project. Covers class usage, layering conventions, and token naming."
applyTo: "**/*.{html,css,js,ts,vue}"
---

# Tailwind style guide

## Utility classes

### Layout utilities — always inline

Flex, grid, gap, and alignment utilities describe **visible structure**. Always apply them as utility classes directly on the element in markup, never inside `@apply`:

```html
<!-- Correct: layout intent is visible in the HTML -->
<div class="flex items-center gap-4">

<!-- Avoid: hides structure behind a class name -->
<div class="my-row">  <!-- .my-row { @apply flex items-center gap-4 } -->
```

Utilities in this category include (but are not limited to):

- **Display:** `flex`, `grid`, `inline-flex`, `block`, `hidden`
- **Direction / wrap:** `flex-col`, `flex-row`, `flex-wrap`
- **Alignment:** `items-*`, `justify-*`, `self-*`, `place-*`
- **Gap:** `gap-*`, `gap-x-*`, `gap-y-*`
- **Grid structure:** `grid-cols-*`, `col-span-*`, `row-span-*`

<!-- TODO: Add guidance on preferred shorthand utilities (e.g., p-4 over px-4 py-4 when sides are equal) -->
<!-- TODO: Clarify when to extract repeated class groups vs leaving them inline -->
<!-- TODO: Note any utilities that are off-limits (e.g., avoid arbitrary values unless necessary) -->

## Responsive design

<!-- TODO: Document the mobile-first convention (default = mobile, sm:/md:/lg: for larger) -->
<!-- TODO: Set expectations on which breakpoints are in active use for this project -->

## Layer usage

| Layer | What goes here |
| --- | --- |
| `@layer base` | Element-level defaults (`body`, `html`); all token definitions (`:root`, `.dark`, `.theme-*` selector blocks) |
| `@layer components` | Named, reusable component classes using `@apply` and `var()` references; BEM naming |
| `@layer utilities` | Single-purpose helpers not covered by a built-in utility; rarely needed — prefer a Tailwind utility where one exists |

Put all theme and mode color tokens in `@layer base`, not in `@layer components`. Component classes should reference `var(--color-*)` and contain zero theme-specific or `dark:` utility prefixes.

## Tailwind vs. components

| Situation | Recommendation |
| --- | --- |
| Page layout | Use Tailwind utilities directly |
| Spacing between local elements | Use Tailwind utilities directly |
| Repeated button/card/form patterns | Create components |
| Complex component internals | BEM naming |
| Highly reused variants | Use props, not copy/paste |
| Global visual system | Define Tailwind tokens |
| Deep child styling | Avoid; use slots/props/classes instead |
| Scoped CSS | Optional, but not the foundation |

**Use Tailwind for:** layout, spacing, responsive behavior, visual tokens, rapid composition.

**Use components for:** buttons, cards, alerts, forms, navigation, repeated content patterns, anything with variants or business meaning.

## @apply

Use `@apply` when:

- You are defining a named, reusable component class in `@layer components` — not a one-off style.
- The utility chain is long or repeated across multiple elements within the same component.
- The abstraction genuinely improves readability and makes the component's structure clearer.

Avoid `@apply` when:

- The style is applied in only one place — inline utilities are easier to read and trace.
- It hides layout structure that should be visible in the markup. **Never use `@apply` for layout utilities** (`flex`, `grid`, `gap-*`, `items-*`, `justify-*`, `grid-cols-*`, etc.) — these belong in the HTML so that the structural intent is immediately readable.
- It substitutes for a Vue (or other framework) component — if a pattern recurs across files, extract a component instead.
- It starts to form a new utility system — that rebuilds the bespoke CSS layer Tailwind is designed to replace.

Where naming aids structure within a component, use BEM: `block__element--modifier` (e.g., `.card__title--featured`). Always place `@apply` inside `@layer components`, never `@layer utilities`.

## Token and custom property naming

All color custom properties use a `--color-` prefix. The standard vocabulary for any theme:

| Token | Role |
| --- | --- |
| `--color-surface` | Main page background |
| `--color-surface-raised` | Elevated surfaces (cards, panels) |
| `--color-surface-sunken` | Recessed surfaces (inputs, code backgrounds) |
| `--color-text-primary` | Body copy and headings |
| `--color-text-secondary` | Supporting text, captions |
| `--color-text-muted` | Placeholder, disabled, metadata text |
| `--color-border` | Card edges, dividers |
| `--color-accent` | Primary interactive / brand color |
| `--color-accent-foreground` | Text placed directly on the accent color |

### Defining tokens

Declare all tokens inside `@layer base`. The `:root` block provides a fallback (typically the default light theme). Scoped class selectors override the same names for other themes or modes:

```css
@layer base {
  :root,
  .theme-ocean {
    --color-surface: #ffffff;
    --color-accent: #2563eb;
    /* ... */
  }

  .theme-ocean.dark {
    --color-surface: #172554;
    --color-accent: #60a5fa;
    /* ... */
  }
}
```

### Referencing tokens

Always use `var()` in component CSS — never hard-code raw color values. Prefer `var()` over the `theme()` function; `var()` is the standard in Tailwind v4.

```css
/* Correct */
.token-card {
  background-color: var(--color-surface-raised);
  border-color: var(--color-border);
}

/* Avoid */
.token-card {
  background-color: #f9fafb; /* hard-coded; breaks on theme switch */
}
```

### Theming preference — semantic tokens over per-theme variants

When building multi-theme or light/dark systems, **prefer the semantic token approach** (lessons 05–06) over per-theme `@variant` declarations (lesson 07).

**Prefer — semantic tokens:**

```css
/* @layer base defines values; components stay clean */
.my-card {
  background-color: var(--color-surface-raised); /* works for all themes */
  border-color: var(--color-border);
}
```

**Avoid — per-theme variant prefixes:**

```css
/* Requires all four prefixes for every color property; doesn't support subtree scoping */
.my-card {
  @apply ocean:bg-blue-50 ocean-dark:bg-blue-900 forest:bg-green-50 forest-dark:bg-green-900;
  @apply ocean:border-blue-200 ocean-dark:border-blue-800 forest:border-green-200 forest-dark:border-green-800;
}
```

The token approach scales to additional themes with a single new `@layer base` block and zero component changes. Per-theme variants require new `@apply` lines on every color-bearing property and cannot scope themes to a subtree using CSS custom property inheritance.

## What to avoid

- **Hard-coded color values in component CSS** — use `var(--color-*)` tokens; hard-coded values break when themes or modes switch.
- **Per-theme `@variant` declarations for color theming** (e.g., `@variant ocean (.theme-ocean &)`) — they prevent subtree scoping, require every color-bearing property to carry multiple prefixes, and scale poorly as themes are added. Use semantic tokens instead.
- **`dark:` utility prefixes on component classes** — centralize all dark-mode color changes in `@layer base`; components should need no `dark:` prefixes at all.
- **`theme()` function** — use `var()` in Tailwind v4; `theme()` is a v3 pattern.
- **`!important`** — specificity problems are a signal to fix the selector, not force an override.
- **Inline `style` attributes for color** — these bypass the token system and are invisible to theme switching.
