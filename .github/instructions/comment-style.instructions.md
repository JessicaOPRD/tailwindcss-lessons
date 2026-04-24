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

## What to comment

- Comment **why**, not **what**. Avoid restating what the code visually says.
- Add a comment when a non-obvious value is used, a workaround is in place, or the intent won't be clear in 6 months.
