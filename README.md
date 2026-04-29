# Tailwind CSS exploration

My first hands-on exploration of Tailwind CSS v4, built in collaboration with an AI agent (Claude Sonnet 4.6). The goal was to work through concepts I regularly encounter — theming, dark mode, responsive design, component abstraction — and understand how Tailwind approaches each one.

## Structure

```
01-cdn/           CDN-based examples — no build step required
02-cli-build/     CLI build examples using the Tailwind v4 CLI and Prettier
```

The `01-cdn/` track is for quick throwaway experiments. The `02-cli-build/` track is the main learning path; each numbered folder is a self-contained lesson with its own `input.css` and `index.html`.

## Lessons

| Folder | Topic | Key idea |
| --- | --- | --- |
| `01-utilities/` | Utility-first basics | Everything inline; minimal CSS |
| `02-layers/` | `@layer` components | Semantic class names; HTML stays clean |
| `03-brand-palette/` | Brand palette | Full `--color-*` scale in `@theme`; semantic aliases |
| `04-theme-overrides/` | Theme overrides | Patching built-in tokens (`--spacing`, `--radius-*`) |
| `05-dark-mode/` | Dark mode | Class-based toggle via `@variant dark (.dark &)` |
| `06-multi-theme-tokens/` | Multi-theme tokens | Semantic CSS variables for two themes × two modes |
| `07-multi-theme-utilities/` | Multi-theme utilities | Same four states using custom `@variant` prefixes instead of tokens |
| `08-breakpoints/` | Responsive design | Mobile-first scale, custom breakpoints, reading breakpoints in JS |

## Running a lesson

From `02-cli-build/`:

```bash
npm install
npm run build           # builds all lessons
npm run build:dark-mode # builds one lesson
npm run watch:dark-mode # rebuilds on file change
```

Open the lesson's `index.html` directly in a browser, or run a local server from the repo root:

```bash
npm install  # root package.json
npm run dev  # serves on port 5500
```

## Formatting

Prettier with `prettier-plugin-tailwindcss` enforces canonical class order across all files. From `02-cli-build/`:

```bash
npm run format
```

See `02-cli-build/README.md` for VS Code format-on-save setup.

## Takeaways

**Don't over-rely on `@apply`.** It works against the grain of the framework. Extracting utilities into `@apply` blocks recreates bespoke CSS — the very thing Tailwind is designed to replace. Reserve it for genuinely reusable component classes in `@layer components`, not one-off styles.

**Use Tailwind in projects where you can build reusable components.** Utility classes shine when a component encapsulates them. Without a component layer (Vue, React, etc.), long class strings repeat across markup. Lessons `01` and `02` make this trade-off concrete.

**Ask the agent to consult the latest official docs.** Tailwind v4 changed significantly from v3 — `@theme`, `@variant`, `var()` over `theme()`, and the new CSS-first config model. When generating lesson content, explicitly prompting the agent to reference the current docs at [tailwindcss.com/docs](https://tailwindcss.com/docs) avoids v3 patterns creeping in.
