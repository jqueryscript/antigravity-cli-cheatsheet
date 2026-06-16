# Antigravity CLI Cheatsheet

A compact reference for Google Antigravity CLI (`agy`): install commands, slash commands, shortcuts, settings, permissions, subagents, plugins, MCP, and Gemini CLI migration.

## Contents

- [Install](#install)
- [Quick reference](#quick-reference)
- [Slash commands](#slash-commands)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Settings and paths](#settings-and-paths)
- [Permissions and sandbox](#permissions-and-sandbox)
- [Subagents, plugins, skills, and MCP](#subagents-plugins-skills-and-mcp)
- [Gemini CLI migration](#gemini-cli-migration)
- [Official sources](#official-sources)

## Install

### macOS and Linux

```bash
curl -fsSL https://antigravity.google/cli/install.sh | bash
```

Installs to:

```text
~/.local/bin/agy
```

### Windows PowerShell

```powershell
irm https://antigravity.google/cli/install.ps1 | iex
```

### Windows CMD

```cmd
curl -fsSL https://antigravity.google/cli/install.cmd -o install.cmd && install.cmd && del install.cmd
```

Windows installs `agy` under:

```text
C:\Users\<Username>\AppData\Local\agy\bin
```

### Install flags

| Flag | Purpose |
|---|---|
| `--skip-aliases` | Keep existing aliases. |
| `--skip-path` | Do not edit `PATH`. |

## Quick reference

| Task | Command |
|---|---|
| Start CLI | `agy` |
| Show slash commands | `/` |
| Help | `?` or `/usage` |
| Settings | `/config` or `/settings` |
| Permissions | `/permissions` |
| Show diffs | `/diff` |
| Resume session | `/resume` |
| Rewind session | `/rewind` |
| Fork session | `/fork` |
| Add directory | `/add-dir <path>` |
| Run shell command | `!<command>` |
| Subagent panel | `/agents` |
| Task logs | `/tasks` |
| Skills | `/skills` |
| MCP manager | `/mcp` |
| Hooks | `/hooks` |
| Log out | `/logout` |
| Exit | `/exit` |

## Slash commands

| Command | Use |
|---|---|
| `/add-dir <path>` | Add workspace folder. |
| `/agents` | Open subagent panel. |
| `/btw <query>` | Ask side question. |
| `/clear` | Clear terminal/context. |
| `/config` (`/settings`) | Open settings. |
| `/diff` | Show file diffs. |
| `/exit` | Close CLI. |
| `/fast` | Enable fast mode. |
| `/fork` (`/branch`) | Fork session. |
| `/hooks` | View hooks. |
| `/keybindings` | Edit shortcuts. |
| `/logout` | Clear saved tokens. |
| `/mcp` | Manage MCP servers. |
| `/model` | Choose model. |
| `/open <path>` | Open file. |
| `/permissions` | Set approvals. |
| `/planning` | Enable planning mode. |
| `/rename <name>` | Rename thread. |
| `/resume` (`/switch`, `/conversation`) | Resume session. |
| `/rewind` (`/undo`) | Roll back history. |
| `/skills` | Browse skills. |
| `/statusline` | Edit status bar. |
| `/tasks` | View shell logs. |
| `/title [on/off]` | Set terminal title. |
| `/usage` | Open help manual. |

## Keyboard shortcuts

Use `/keybindings` to inspect or edit active shortcuts.

| Shortcut | Action |
|---|---|
| `Enter` | Submit. |
| `Shift+Enter`, `Ctrl+J`, `Alt+Enter` | Newline. |
| `Esc` | Close, stop, or clear. |
| `Ctrl+C` | Cancel or exit. |
| `Ctrl+D` | Exit CLI. |
| `Ctrl+L` | Clear screen. |
| `Ctrl+V` | Paste. |
| `Ctrl+G` | Open editor. |
| `Ctrl+O` | Toggle details. |
| `Ctrl+R` | Review artifacts. |
| `Alt+J` | Jump to subagent. |
| `Ctrl+K` | Approve subagent action. |
| `Ctrl+A` | Cursor start. |
| `Ctrl+E` | Cursor end. |
| `Ctrl+Z` | Undo text. |
| `Ctrl+Shift+Z` | Redo text. |
| `Up` / `Down` | Move selection. |
| `PgUp` / `Shift+Up` | Scroll up. |
| `PgDn` / `Shift+Down` | Scroll down. |
| `Tab` | Accept autofill. |
| `y` | Approve. |
| `n` | Reject. |
| `A` | Approve all artifacts. |

## Settings and paths

| Path | Purpose |
|---|---|
| `~/.gemini/antigravity-cli/settings.json` | Settings. |
| `~/.gemini/antigravity-cli/keybindings.json` | Keybindings. |
| `~/.gemini/antigravity-cli/plugins/<plugin_name>/` | Plugins. |
| `~/.gemini/antigravity-cli/skills/` | Global skills. |
| `.agents/skills/` | Workspace skills. |
| `~/.gemini/config/mcp_config.json` | Global MCP. |
| `.agents/mcp_config.json` | Workspace MCP. |

| Setting | Default and use |
|---|---|
| `colorScheme` | `"terminal"`; color theme. |
| `altScreenMode` | `"default"`; screen buffer. |
| `toolPermission` | `"request-review"`; approvals. |
| `artifactReviewPolicy` | `"asks-for-review"`; artifact review. |
| `notifications` | `false`; completion alerts. |
| `showTips` | `true`; prompt tips. |
| `showFeedbackSurvey` | `true`; feedback prompts. |
| `editor` | `"auto"`; external editor. |
| `allowNonWorkspaceAccess` | `false`; outside-root access. |
| `enableTerminalSandbox` | `false`; command sandboxing. |
| `enableTelemetry` | `true`; metrics and crash logs. |
| `verbosity` | `"high"`; output detail. |
| `runningLightSpeed` | `"medium"`; progress animation. |

## Permissions and sandbox

| Preset | Behavior |
|---|---|
| `request-review` | Prompt before risky tools. |
| `proceed-in-sandbox` | Auto-proceed in sandbox. |
| `always-proceed` | No prompts. |
| `strict` | Prompt for all non-read tools. |

Enable the terminal sandbox:

```json
{
  "enableTerminalSandbox": true
}
```

## Subagents, plugins, skills, and MCP

Use `/agents` to inspect background subagents. Use `/tasks` for shell logs. Use `/skills` for Agent Skills. Use `/mcp` for Model Context Protocol servers. Use `/hooks` for pre-flight and post-format hooks.

Plugin layout:

```text
~/.gemini/antigravity-cli/
|-- plugins/
|   `-- <plugin_name>/
|       |-- plugin.json
|       |-- mcp_config.json
|       |-- hooks.json
|       |-- skills/
|       |-- agents/
|       `-- rules/
`-- import_manifest.json
```

## Gemini CLI migration

Run first launch onboarding by starting:

```bash
agy
```

Manual extension import:

```bash
agy plugin import gemini
```

Context files:

```text
GEMINI.md
AGENTS.md
~/.gemini/GEMINI.md
```

Skills path changes:

| Scope | Path change |
|---|---|
| Global skills | `~/.gemini/skills/` to `~/.gemini/antigravity-cli/skills/` |
| Workspace skills | `.gemini/skills/` to `.agents/skills/` |

MCP config changes:

| Gemini CLI | Antigravity CLI |
|---|---|
| `~/.gemini/settings.json` | `~/.gemini/config/mcp_config.json` |
| Inline MCP server definitions | Standalone `mcp_config.json` profiles |
| `url` or `httpUrl` | `serverUrl` |

## Official sources

- [Antigravity CLI Overview](https://antigravity.google/docs/cli-overview)
- [Installation & Auth](https://antigravity.google/docs/cli-install)
- [Using AGY CLI](https://antigravity.google/docs/cli-using)
- [Antigravity CLI Features](https://antigravity.google/docs/cli-features)
- [Gemini Migration](https://antigravity.google/docs/gcli-migration)
- [CLI Reference](https://antigravity.google/docs/cli-reference)

## More ScriptByAI cheatsheets and resources

- [Claude Code Commands Cheat Sheet](https://www.scriptbyai.com/claude-code-commands-cheat-sheet/)
- [OpenAI Codex Commands Cheat Sheet](https://www.scriptbyai.com/codex-commands-cheat-sheet/)
- [The Ultimate Claude Code Resource List](https://www.scriptbyai.com/claude-code-resource-list/)
- [160+ Most Popular Agent Skills on GitHub for Coding Agents](https://www.scriptbyai.com/most-popular-agent-skills/)
