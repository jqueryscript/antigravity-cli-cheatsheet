# Antigravity CLI Cheatsheet

A compact reference for Google Antigravity CLI (`agy`): install commands, slash commands, shortcuts, settings, permissions, subagents, plugins, MCP, and Gemini CLI migration.

Updated for Antigravity CLI 1.1.5 on July 21, 2026.

## Contents

- [Install](#install)
- [Quick reference](#quick-reference)
- [What changed in 1.1.5](#what-changed-in-115)
- [Launch flags and headless mode](#launch-flags-and-headless-mode)
- [Slash commands](#slash-commands)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Execution modes](#execution-modes)
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
| Search code | `/codesearch`, `/cs`, `/search` |
| Help | `?` or `/usage` |
| Settings | `/config` or `/settings` |
| Permissions | `/permissions` |
| Model | `/model`, `--model <slug>` |
| Reasoning effort | `/effort`, `/effort <level>`, `--effort <level>` |
| Plan mode | `/plan` |
| Cycle execution mode | `Shift+Tab` |
| Show diffs | `/diff` |
| Resume session | `/resume` |
| Rewind session | `/rewind` |
| Fork session | `/fork` |
| Add directory | `/add-dir <path>` |
| Run shell command | `!<command>` |
| Print mode | `agy -p "<prompt>"` |
| Custom agent | `agy --agent <name>` |
| Subagent panel | `/agents` |
| Task logs | `/tasks` |
| Skills | `/skills` |
| MCP manager | `/mcp` |
| Hooks | `/hooks` |
| Log out | `/logout` |
| Exit | `/exit` |

## What changed in 1.1.5

| Change | What it means |
|---|---|
| `/effort` | View or change reasoning effort without restarting the CLI. |
| `--effort <level>` | Choose a reasoning-effort variant when launching. |
| Model slugs | Use the stable model names shown in `/model` with `--model`. |
| Custom-agent model | Add `model` to agent frontmatter to select a subagent tier; omit it to inherit the parent model. |
| Command chains | Combine leading slash commands such as `/plan /grill-me` in one prompt. |
| `/diff` | Scrolling remains stable when lines wrap or comments expand. |

## Launch flags and headless mode

| Command or flag | Use |
|---|---|
| `agy -p "<prompt>"` | Run once and print the result. |
| `--model <slug>` | Select a model with a stable model slug. |
| `--effort <level>` | Select the model's reasoning-effort variant at launch. |
| `--mode <mode>` | Start in default, accept-edits, or plan mode. |
| `--agent <name>` | Start with a custom agent. |
| `agy agent`, `agy agents` | List available custom agents. |
| `--project <project>` | Open an existing project. |
| `--new-project <name>` | Create a project. |
| `--sandbox` | Enable sandboxing for the session. |
| `AGY_CLI_CMD_OUTPUT_PERCENTAGE` | Limit command output shown in the TUI. |

Print mode returns failures through stderr with a nonzero exit code. Tools that require approval are denied unless a matching allow rule exists, so scripts do not hang on an interactive confirmation.

## Slash commands

| Command | Use |
|---|---|
| `/add-dir <path>` | Add workspace folder. |
| `/agents` | Open subagent panel. |
| `/btw <query>` | Ask side question. |
| `/clear` | Clear terminal/context. |
| `/config` (`/settings`) | Open settings. |
| `/codesearch` (`/cs`, `/search`) | Search workspace code. |
| `/diff` | Show file diffs. |
| `/exit` | Close CLI. |
| `/fork` (`/branch`) | Fork session. |
| `/hooks` | View hooks. |
| `/keybindings` | Edit shortcuts. |
| `/logout` | Clear saved tokens. |
| `/mcp` | Manage MCP servers. |
| `/model` | Choose model. |
| `/effort` (`/effort <level>`) | View or set reasoning effort. |
| `/open <path>` | Open file. |
| `/permissions` | Set approvals. |
| `/plan` | Enter plan mode. |
| `/rename <name>` | Rename thread. |
| `/resume` (`/switch`, `/conversation`) | Resume session. |
| `/rewind` (`/undo`) | Roll back history. |
| `/skills` | Browse skills. |
| `/statusline` | Edit status bar. |
| `/tasks` | View shell logs. |
| `/title [on/off]` | Set terminal title. |
| `/usage` | Open help manual. |

`/codesearch` uses regular expressions by default. Add `-F` or `--literal` for exact text. Use `f:` or `file:` globs to include or exclude paths.

## Keyboard shortcuts

Use `/keybindings` to inspect or edit active shortcuts.

| Shortcut | Action |
|---|---|
| `Enter` | Submit. |
| `Shift+Enter`, `Ctrl+J`, `Alt+Enter` | Newline. |
| `Shift+Tab` | Cycle execution modes. |
| `Esc` | Close, stop, or clear. |
| `Ctrl+C` | Cancel work; press twice to exit. |
| `Ctrl+D` | Forward-delete; exit on empty prompt. |
| `Ctrl+L` | Clear screen. |
| `Ctrl+V` | Paste. |
| `Alt+V` | Alternative paste on Windows. |
| `Ctrl+G` | Open prompt, diff, or confirmation in `$EDITOR`. |
| `Ctrl+O` | Toggle details. |
| `Ctrl+R` | Review artifacts. |
| `f` | Open full diff from file review. |
| `/` in artifact details | Search the current artifact. |
| `n` / `N` during search | Next or previous match. |
| `Shift+N` in diff view | Previous diff. |
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
| `y` / `n` at confirmation | Approve or reject. |
| `A` | Approve all artifacts. |

## Execution modes

Choose Agent Mode in `/settings`, pass `--mode`, or press `Shift+Tab` to cycle modes.

| Mode | Behavior |
|---|---|
| `default` | Review file writes before they run. |
| `accept-edits` | Accept file edits automatically. |
| `plan` | Plan without applying edits. |

## Settings and paths

| Path | Purpose |
|---|---|
| `~/.gemini/antigravity-cli/settings.json` | Settings. |
| `~/.gemini/antigravity-cli/keybindings.json` | Keybindings. |
| `~/.gemini/antigravity-cli/plugins/<plugin_name>/` | Plugins. |
| `~/.gemini/antigravity-cli/skills/` | Global skills. |
| `~/.gemini/config/` | Global agents. |
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

Use `permission.allow` to preapprove file writes or commands. Command rules use strict matching by default; prefix a rule with `regex:` only when a regular expression is required.

Enable the terminal sandbox:

```json
{
  "enableTerminalSandbox": true
}
```

## Subagents, plugins, skills, and MCP

Use `/agents` to inspect background subagents, including nested subagents. Use `/tasks` for shell logs. Use `/skills` for Agent Skills. Use `/mcp` for Model Context Protocol servers. Use `/hooks` for pre-flight and post-format hooks.

Use `--agent <name>` to select a custom agent at launch. The `agent` and `agents` subcommands list available agents. Add `model` to custom-agent frontmatter when a subagent should use a selected model tier; otherwise it inherits the parent model.

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
