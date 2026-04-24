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

## Language-specific syntax

**HTML**
```html
<!-- Single-line comment -->

<!--
  Multi-line comment.
  Indent continuation lines.
-->

<!-- #region Section Name -->
...
<!-- #endregion -->
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

Rules:
- `APPROACH {n}:` — all-caps label, consistent with the numbered lesson sequence.
- Title line — title case after the colon; em dash separator between technology and short title.
- `===` underline — a solid line of equals signs running to the same visual width as the title line. Fixed length is fine here; this is a human-readable banner, not auto-maintained code.
- Body — sentence case; use `—` (em dash) for inline asides, `…` for elided ranges.
- Run instruction — always the last line, one blank line above it.

**CSS lesson header** — placed at the very top of `input.css`, before `@import`:

```css
/*
  {Technology} – {Lesson folder name, title case}
  ==================================================
  One or two sentences describing what this lesson demonstrates.

  Concepts covered:
  • First concept  — description
  • Second concept — description

  Run from {folder}/:
    npm run {build-script}
    npm run {watch-script}
*/
```

Rules mirror the HTML form above; use `•` bullets instead of numbered list for CSS files.

**Sub-lesson concept block** — used inside any CSS file to introduce a named concept within a lesson (e.g. a major `@theme` token group). Placed immediately before the relevant block of declarations:

```css
/*
  CONCEPT NAME
  ───────────────────────────────────────────────
  One or two sentences explaining the concept and
  why it matters. Sentence case; full stops on
  complete sentences.
*/
```

Rules:
- Label line — all-caps, no trailing punctuation.
- `───` underline — a solid line of box-drawing characters running to roughly the same visual width as the label. Fixed length is fine; this is a human-readable banner.
- Body — sentence case; explain *why*, not *what*.

## Section headers

The banner lines are fixed, copyable strings — never fill the label line with trailing dashes or spaces to reach a target width. Fill-to-width patterns are hard to maintain (every label edit requires recounting) and fragile to automate.

The top banner line is 80 characters, matching Prettier's default `printWidth`. The label line has no trailing padding or closing token.

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

Always: uppercase tag → colon → space → description.

```
// TODO: Add mobile breakpoint handling
// FIXME: Border radius overrides broken on Safari
// NOTE: This value must match the backend API enum
// HACK: Temporary workaround until design tokens are finalized
```

## Language

- Use American English spelling in all comments (e.g. "color" not "colour", "customize" not "customise").

## What to comment

- Comment **why**, not **what**. Avoid restating what the code visually says.
- Add a comment when a non-obvious value is used, a workaround is in place, or the intent won't be clear in 6 months.
