# Instructions files

This folder contains `.instructions.md` files that provide coding guidelines and
style rules to AI agents. Each file focuses on a single concern and can be scoped
to specific file types.

> **Note:** The frontmatter properties described below (`applyTo`, `description`,
> `name`) are specific to the **GitHub Copilot extension for VS Code**. They have
> no effect on GitHub.com Copilot (e.g., PR summaries or Copilot Chat on github.com).
> For instructions that apply on GitHub.com as well, use `.github/copilot-instructions.md`.

---

## Frontmatter reference

| Property | Required | Description |
|---|---|---|
| `description` | No | Short summary used for on-demand discovery. The agent reads this to decide whether the file is relevant to the current task. Use keyword-rich "Use when..." phrasing. |
| `applyTo` | No | Glob pattern. When a file matching this pattern is in context, the instructions are automatically injected. Omit for on-demand-only loading. |
| `name` | No | Display name shown in the VS Code Chat Customizations UI. Defaults to the filename. |

---

## Discovery modes

| Mode | Setup | Behavior |
|---|---|---|
| **Always-on** | `applyTo` set to a glob | Injected automatically when a matching file is open or being edited |
| **On-demand** | `description` only, no `applyTo` | Agent loads it when it determines the task is relevant |
| **Manual** | Neither property set | Only loads when explicitly added via **Add Context → Instructions** in the Chat view |

---

## Tips

- Keep each file focused on one concern (naming, comments, testing, etc.).
- Avoid `applyTo: "**"` unless the rule truly applies to every file — it consumes context on every request.
- Prefer specific globs: `**/*.{html,css,js,ts,vue}` over `**`.
- Use the **Chat Customizations editor** (`Chat: Open Chat Customizations` in the Command Palette) to view and manage all loaded instruction files.
- Use the **Diagnostics** view (right-click in Chat → Diagnostics) to verify which files are active and catch frontmatter errors.

See the [official VS Code documentation](https://code.visualstudio.com/docs/copilot/customization/custom-instructions) for full details.
