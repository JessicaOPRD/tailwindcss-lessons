# Copilot instructions

## Project context

This is a **Tailwind CSS v4 learning repository** with two tracks:

- `01-cdn/` — CDN-based examples (no build step)
- `02-cli-build/` — CLI build examples using the Tailwind v4 CLI and Prettier

Lessons progress through utilities, layers, and brand palette configuration.

---

## Tech stack

- **Tailwind CSS v4** — utility-first CSS framework
- **Tailwind CLI** (`@tailwindcss/cli`) — for building CSS from `input.css` files
- **Prettier** + `prettier-plugin-tailwindcss` — code formatting
- Plain HTML and CSS; no JavaScript framework

---

## Code style

### Comments

- One space after the comment marker; capitalize the first word
- Comment **why**, not what — avoid restating what the code visually says
- Annotation tags: `/* TODO: */`, `/* FIXME: */`, `/* NOTE: */` — uppercase, colon, space
- Section headers: `/* === Section Name === */`

> Detailed syntax rules live in `.github/instructions/comment-style.instructions.md` (VS Code only).

### CSS / HTML

<!-- TODO: Add project CSS conventions (e.g., property order, shorthand preferences) -->
<!-- TODO: Add HTML formatting preferences (e.g., attribute order, self-closing tags) -->

---

## Naming conventions

<!-- TODO: Add file and folder naming conventions -->
<!-- TODO: Add CSS custom property naming conventions (e.g., --color-brand-primary) -->

---

## Tailwind-specific guidelines

<!-- TODO: Add preferences around @layer usage (base / components / utilities) -->
<!-- TODO: Add guidance on when to use @apply vs inline utilities -->
<!-- TODO: Add any token/palette naming conventions for theme() or CSS variables -->

---

## Formatting

- Formatter: **Prettier** with `prettier-plugin-tailwindcss`
- Run: `npm run format` from `02-cli-build/`
- Class order is enforced automatically by the Prettier plugin — do not reorder manually

---

## What Copilot should avoid

<!-- TODO: List patterns to avoid (e.g., inline styles, !important, deprecated utilities) -->
