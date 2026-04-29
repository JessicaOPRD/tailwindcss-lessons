---
description: "Use when writing or reviewing Tailwind CSS utility classes, @layer rules, CSS custom properties, or variant declarations in this project. Covers class usage, layering conventions, token naming, and the @custom-variant / @variant distinction."
applyTo: "**/*.{html,css,js,ts,vue}"
---

# Tailwind style guide

## Contexts

This guide applies to all Tailwind projects. A few sections — `@apply`, class naming, and `@layer components` — behave differently depending on whether the project uses a component architecture (Vue, React) or plain HTML with no build-time component layer. Those differences are called out in each section.

| Section | Plain HTML | Vue / React |
| --- | --- | --- |
| `@layer components` + `@apply` | Primary pattern for reusable styles | Rarely needed; the component file is the encapsulation unit |
| Class naming | BEM recommended for multi-element patterns | Block prefix redundant; `__` and `--` notation still useful in complex components — see BEM-inspired naming section |
| Tokens + `@custom-variant` | Yes | Yes |
| Component extraction | Manual — repeated markup is the signal | Framework components with props and slots |

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
| `@layer components` | Named, reusable component classes using `@apply` and `var()` references; BEM naming (plain HTML). In Vue / React, reserve for third-party library overrides — component templates are the encapsulation unit. |
| `@layer utilities` | Single-purpose helpers not covered by a built-in utility; rarely needed — prefer a Tailwind utility where one exists |

Put all theme and mode color tokens in `@layer base`, not in `@layer components`. Component classes should reference `var(--color-*)` and contain zero theme-specific or `dark:` utility prefixes.

## Tailwind vs. components

| Situation | Recommendation |
| --- | --- |
| Page layout | Use Tailwind utilities directly |
| Spacing between local elements | Use Tailwind utilities directly |
| Repeated button/card/form patterns | Create components |
| Complex component internals (plain HTML) | BEM naming |
| Complex component internals (Vue / React) | Scoped styles or CSS Modules; `__` and `--` notation useful in complex components; plain names fine in simple ones |
| Highly reused variants | Use props, not copy/paste |
| Global visual system | Define Tailwind tokens |
| Deep child styling | Avoid; use slots/props/classes instead |
| Scoped CSS | Optional, but not the foundation |

**Use Tailwind for:** layout, spacing, responsive behavior, visual tokens, rapid composition.

**Use components for:** buttons, cards, alerts, forms, navigation, repeated content patterns, anything with variants or business meaning.

## @apply

**In all contexts:** never use `@apply` for layout utilities (`flex`, `grid`, `gap-*`, `items-*`, `justify-*`, `grid-cols-*`, etc.) — these belong in the markup where structural intent is immediately readable.

### Plain HTML

Use `@apply` in `@layer components` to name and reuse patterns that recur across multiple pages or sections when no component layer is available to encapsulate them.

Use `@apply` when:

- You are defining a named, reusable class in `@layer components` — not a one-off style.
- The utility chain is long or repeated across multiple elements.
- The abstraction genuinely improves readability.

Avoid `@apply` when:

- The style is applied in only one place — inline utilities are easier to read and trace.
- It starts to form a new utility system — that rebuilds the bespoke CSS layer Tailwind is designed to replace.

For naming, use BEM: `block__element--modifier` (e.g., `.card__title--featured`). Always place `@apply` inside `@layer components`, never `@layer utilities`.

### Vue / React

`@apply` is not needed in scoped component styles. The component template is the encapsulation unit — write styles directly using `var(--color-*)` tokens; there is no need to name utility groups with `@apply`.

Reserve `@apply` for:

- Element defaults in `@layer base` (e.g., resetting `body`, `h1`).
- Third-party library overrides where you cannot control the markup.

### BEM-inspired naming in scoped components

In scoped styles (Vue `<style scoped>`, CSS Modules), the component file provides the block namespace — the block prefix is redundant. Use only the **element** (`__`) and **modifier** (`--`) conventions from BEM. Reference the global token system via `var(--color-*)` directly.

**Element — `__`**

An element is a sub-part of the component that has no meaning on its own. Use flat class selectors — never nest under a parent:

```css
/* Correct — flat selectors, var() for tokens */
.label { font-size: 0.875rem; font-weight: 500; color: var(--color-text-primary); }
.input { border: 1px solid var(--color-border); border-radius: 0.375rem; padding: 0.5rem 0.75rem; }
.hint  { font-size: 0.75rem; color: var(--color-text-muted); }

/* Avoid — nesting adds specificity and is unnecessary in scoped styles */
.control .input { ... }
```

Use `__` prefixing when a component has enough sub-elements that grouping them aids readability in the stylesheet:

```css
/* Flat component — plain names are fine */
.label { color: var(--color-text-primary); }
.input { border-color: var(--color-border); }
.hint  { color: var(--color-text-muted); }

/* Complex component — __ grouping makes structure readable without opening the template */
.field__label   { color: var(--color-text-primary); }
.field__input   { border-color: var(--color-border); }
.field__hint    { color: var(--color-text-muted); }
.field__counter { color: var(--color-text-muted); }
```

The threshold is judgment — roughly four or more named sub-elements that benefit from visible grouping.

**Modifier — `--`**

A modifier changes the appearance, state, or behavior of an element. It is always an **additional** class alongside the base class — never applied alone:

```html
<!-- Correct — base class + modifier together -->
<input class="input input--error" />
<span class="hint hint--error">Required</span>

<!-- Avoid — modifier alone loses the base styles -->
<input class="input--error" />
```

```css
.input        { border: 1px solid var(--color-border); border-radius: 0.375rem; }
.input--error { border-color: var(--color-error); }

.hint         { font-size: 0.75rem; color: var(--color-text-muted); }
.hint--error  { color: var(--color-error); }
```

**What to avoid in scoped styles**

- Tag selectors combined with class names (`.form input` instead of `.input`) — fragile and specificity-raising
- Nesting parent selectors to target children — use flat class selectors
- `@apply` — reference `var(--color-*)` tokens directly instead
- Deeply chained modifiers encoding multiple states in one name (`.input--error--large--disabled`) — use separate modifier classes

## Variants

Tailwind v4 has two separate directives for variants. They serve different roles and appear in different places.

### `@custom-variant` — declaring variants

Use `@custom-variant` at the top level to register a new variant or override a built-in one. This is the canonical v4 API for variant declarations.

```css
/* Recommended form for class-based dark mode (official docs) */
@custom-variant dark (&:where(.dark, .dark *));

/* Data-attribute form */
@custom-variant dark (&:where([data-theme=dark], [data-theme=dark] *));
```

The `&:where(.dark, .dark *)` selector is preferred over the older `.dark &` form for two reasons:

- `:where()` contributes **zero specificity**, so dark variant styles never win a specificity fight they shouldn't.
- It matches both the element that **carries** `.dark` and its descendants. `.dark &` only matches descendants — if you scope dark mode to a subtree rather than `<html>`, the root element of that subtree is missed.

`@variant dark (.dark &)` (used in lesson 05) predates `@custom-variant` and is no longer the documented API for declarations. It functions but should not be used in new CSS.

### `@variant` — applying variants inside rules

Use `@variant` inside a CSS rule block to apply an existing variant to custom styles. This is its only correct use:

```css
.my-element {
  background: white;
  @variant dark {
    background: black;
  }
}
```

Nest `@variant` calls to stack multiple variants at the same time:

```css
.my-element {
  background: white;
  @variant dark {
    @variant hover {
      background: black;
    }
  }
}
```

`@variant` used at the top level (outside a rule) is not the documented API — use `@custom-variant` there instead.

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

When building multi-theme or light/dark systems, **prefer the semantic token approach** (lessons 05–06) over per-theme `@custom-variant` declarations (lesson 07).

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
- **Per-theme `@custom-variant` declarations for color theming** (e.g., `@custom-variant ocean (&:where(.theme-ocean, .theme-ocean *))`) — they prevent subtree scoping, require every color-bearing property to carry multiple prefixes, and scale poorly as themes are added. Use semantic tokens instead.
- **`dark:` utility prefixes on component classes** — centralize all dark-mode color changes in `@layer base`; components should need no `dark:` prefixes at all.
- **`@variant` at the top level for variant declarations** — use `@custom-variant` instead; `@variant` belongs inside rule blocks only.
- **`@apply` in scoped component styles** — reference `var(--color-*)` tokens directly; `@apply` in components recreates the abstraction layer the component itself already provides.
- **`theme()` function** — use `var()` in Tailwind v4; `theme()` is a v3 pattern.
- **`!important`** — specificity problems are a signal to fix the selector, not force an override.
- **Inline `style` attributes for color** — these bypass the token system and are invisible to theme switching.
