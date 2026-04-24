---
description: "Use when writing or reviewing Tailwind CSS utility classes, @layer rules, or CSS custom properties in this project. Covers class usage, layering conventions, and token naming."
applyTo: "**/*.{html,css,js,ts,vue}"
---

# Tailwind style guide

## Utility classes

<!-- TODO: Add guidance on preferred shorthand utilities (e.g., p-4 over px-4 py-4 when sides are equal) -->
<!-- TODO: Clarify when to extract repeated class groups vs leaving them inline -->
<!-- TODO: Note any utilities that are off-limits (e.g., avoid arbitrary values unless necessary) -->

## Responsive design

<!-- TODO: Document the mobile-first convention (default = mobile, sm:/md:/lg: for larger) -->
<!-- TODO: Set expectations on which breakpoints are in active use for this project -->

## Layer usage

<!-- TODO: Define what belongs in @layer base (resets, element defaults) -->
<!-- TODO: Define what belongs in @layer components (reusable patterns composed with @apply) -->
<!-- TODO: Define what belongs in @layer utilities (single-purpose overrides not covered by Tailwind) -->

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
- It hides layout structure that should be visible in the markup (e.g., grid or flex relationships).
- It substitutes for a Vue (or other framework) component — if a pattern recurs across files, extract a component instead.
- It starts to form a new utility system — that rebuilds the bespoke CSS layer Tailwind is designed to replace.

Where naming aids structure within a component, use BEM: `block__element--modifier` (e.g., `.card__title--featured`). Always place `@apply` inside `@layer components`, never `@layer utilities`.

## Token and custom property naming

<!-- TODO: Document the naming pattern for design tokens (e.g., --color-brand-primary, --spacing-section) -->
<!-- TODO: Clarify whether theme() or var() is preferred for referencing tokens in CSS -->
<!-- TODO: Add palette naming conventions introduced in 03-brand-palette/ -->

## What to avoid

<!-- TODO: List anti-patterns (e.g., inline style attributes, !important, mixing Tailwind v3 config syntax) -->
