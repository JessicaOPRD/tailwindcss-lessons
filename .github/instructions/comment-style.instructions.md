---
description: "Use when writing or reviewing inline comments, code annotations, section headers, or TODO/FIXME/NOTE tags in any file. Covers formatting, capitalization, and language-specific syntax."
applyTo: "**/*.{html,css,js,ts,vue}"
---

# Inline comment style guide

## General formatting

- Single space after the comment marker (`//`, `#`, `<!--`)
- Capitalize the first word
- No trailing punctuation on short inline comments; end full sentences with a period
- Keep comments brief — use a block comment when continuation is needed
- Use American English spelling (e.g. "color" not "colour", "customize" not "customise")

## Language-specific syntax

**HTML**
```html
<!-- Single-line comment -->

<!--
  Multi-line comment.
  Indent continuation lines.
-->
```

**CSS / SCSS / Tailwind**
```css
/* Single-line comment */

/*
 * Multi-line block comment.
 * One space after the leading asterisk.
 */

/* -------------------------------------------------------------------------- */
/* Section name
/* -------------------------------------------------------------------------- */
```

**JS / TS / Vue `<script>`**
```js
// Inline comment

/* Block comment for multi-line notes */

/** JSDoc for exported functions and public types */

// -----------------------------------------------------------------------------
// Section name
// -----------------------------------------------------------------------------
```

## Section headers

- Banner lines are fixed, copyable strings — never fill to a target width; fill-to-width patterns are fragile to maintain
- Top banner line is 80 characters, matching Prettier's default `printWidth`
- Label line has no trailing padding or closing token

```css
/* -------------------------------------------------------------------------- */
/* Layout
/* -------------------------------------------------------------------------- */
```

```js
// -----------------------------------------------------------------------------
// Utilities
// -----------------------------------------------------------------------------
```

For groups nested inside a section, use a sub-header. The symmetric `---` tokens are a fixed, copyable string — no length calculation needed:

```css
/* --- Buttons --- */
```

```js
// --- Utilities ---
```

## Annotation tags

- Uppercase tag → colon → space → description

```
// TODO: Add mobile breakpoint handling
// FIXME: Border radius overrides broken on Safari
// NOTE: This value must match the backend API enum
// HACK: Temporary workaround until design tokens are finalized
```

## What to comment

- Comment **why**, not **what**. Avoid restating what the code visually says.
- Add a comment when a non-obvious value is used, a workaround is in place, or the intent won't be clear in 6 months.

## Lesson header comments

Lesson files (any `index.html` or `input.css` under a numbered lesson folder) open with a structured overview block. This is a distinct comment type — its purpose is to orient a human learner, not to annotate code logic.

**HTML lesson header** — placed immediately before `<link rel="stylesheet">` in `<head>`:

```html
<!--
  APPROACH {n}: {Technology} — {Short title} ({key tool(s)})
  ============================================================
  One or two sentences describing what this lesson demonstrates.

  Key ideas:
  1. First concept — short description
  2. Second concept — short description
  3. Third concept — short description

  Run from {folder}/:  npm run {script}
-->
```

- `APPROACH {n}:` — all-caps label, consistent with the numbered lesson sequence.
- Title line — title case after the colon; em dash separator between technology and short title.
- `===` underline — a solid line of equals signs running to the same visual width as the title line. Fixed length is fine here; this is a human-readable banner, not auto-maintained code.
- Body — sentence case; use `—` (em dash) for inline asides, `…` for elided ranges.
- Run instruction — always the last line, one blank line above it.

**CSS lesson header** — placed at the very top of `input.css`, before `@import`:

```css
/*
  {Technology} – {Lesson folder name, Short title}
  ================================================
  One or two sentences describing what this lesson demonstrates.

  Concepts covered:
  • First concept  — description
  • Second concept — description

  Run from {folder}/:
    npm run {build-script}
    npm run {watch-script}
*/
```

- Title line — title case for the lesson folder name; en dash (`–`) separator between technology and lesson name
- `===` underline — a solid line of equals signs matching the visual width of the title line
- Body — sentence case; use `—` (em dash) for inline asides, `…` for elided ranges
- Concepts list — use `•` bullets instead of a numbered list
- Run instruction — always the last lines, one blank line above

**Sub-lesson concept block** — used inside any CSS file to introduce a named concept within a lesson (e.g. a major `@theme` token group). Placed immediately before the relevant block of declarations:

```css
/*
  CONCEPT NAME
  ────────────
  One or two sentences explaining the concept and
  why it matters. Sentence case; full stops on
  complete sentences.
*/
```

- Label line — all-caps, no trailing punctuation.
- `───` underline — a solid line of box-drawing characters running to roughly the same visual width as the label. Fixed length is fine; this is a human-readable banner.
- Body — sentence case; explain *why*, not *what*.
