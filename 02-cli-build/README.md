# Prettier + Tailwind CSS — Setup Guide

## Option 1: Bulk formatter (CLI)

Formats all HTML and CSS files in one shot. Useful for onboarding an existing
codebase or running in a pre-commit hook.

### Install

From this folder (`02-cli-build/`):

```bash
npm install --save-dev prettier prettier-plugin-tailwindcss
```

### Configure

Create a `.prettierrc` file at the root of the project (already present here):

```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

### Run

```bash
npm run format
```

This executes `prettier --write "**/*.html" "**/*.css"` (defined in
`package.json`) and sorts Tailwind utility class lists into canonical order
across every matched file.

---

## Option 2: Format on save (VS Code extensions)

Formats the file you are editing each time you save. Requires the same
`.prettierrc` config above.

### Install extensions

1. Open the Extensions panel (`Ctrl+Shift+X`)
2. Search for and install:
   - **Prettier - Code formatter** (`esbenp.prettier-vscode`) — the VS Code
     runner that reads `.prettierrc` and applies `prettier-plugin-tailwindcss`
   - **Tailwind CSS IntelliSense** (`bradlc.vscode-tailwindcss`) *(optional)* —
     adds autocomplete, hover previews, and class linting

### Enable format on save

Add the following to your VS Code settings (`.vscode/settings.json` or user
settings):

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```

To set Prettier as the formatter for HTML specifically (if you use a different
default elsewhere):

```json
{
  "editor.formatOnSave": true,
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

You can also set it per-file: open an HTML file → right-click → **Format
Document With…** → **Configure Default Formatter** → select **Prettier**.
