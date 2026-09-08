# Antigravity CLI Cheatsheet

A compact reference for Google Antigravity CLI (`agy`): install commands, slash commands, shortcuts, settings, permissions, subagents, plugins, MCP, and Gemini CLI migration.

Updated for Antigravity CLI 1.1.27 on September 8, 2026.

## Contents

- [Install](#install)
- [Quick reference](#quick-reference)
- [What changed in 1.1.6–1.1.11](#what-changed-in-116111)
- [What changed in 1.1.12–1.1.27](#what-changed-in-11121127)
- [Launch flags and headless mode](#launch-flags-and-headless-mode)
- [Authentication](#authentication)
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
| Help | `?` or `/help` |
| Quota usage | `/usage` or `/quota` |
| Settings | `/config` or `/settings` |
| Permissions | `/permissions` |
| Model | `/model`, `--model <slug>` |
| Ask another model once | `/model <name> <prompt>` |
| Reasoning effort | `/effort`, `/effort <level>`, `--effort <level>` |
| Plan mode | `/planning` |
| Cycle execution mode | `Shift+Tab` |
| Voice dictation | `/voice`, `/record`, `F5` |
| Show diffs | `/diff` |
| Copy an earlier response | `/copy <n>` |
| Resume session | `/resume` |
| Rewind session | `/rewind` |
| Fork session | `/fork` |
| Add directory | `/add-dir <path>` |
| Run shell command | `!<command>` |
| Print mode | `agy -p "<prompt>"` |
| Stream prompts | `--input-format stream-json` |
| Structured output | `--output-format json` or `stream-json` |
| List models | `agy models` |
| Custom agent | `agy --agent <name>` |
| Subagent panel | `/agents` |
| Task logs | `/tasks` |
| Skills | `/skills` |
| MCP manager | `/mcp`, `agy mcp` |
| Hooks | `/hooks` |
| Log out | `/logout` |
| Exit | `/exit`, `/quit` |

## What changed in 1.1.6–1.1.11

| Change | What it means |
|---|---|
| Vim editor mode | Enable modal editing for prompts, diff comments, and artifact comments under Editor Mode in `/settings`. |
| `/copy <n>` and `/codesearch` | Copy an earlier response with `/copy <n>`; code-search results stream progressively and `Esc` cancels an active search. |
| Structured print output | Use `--output-format json` or `stream-json`, with optional `--json-schema` for a fixed result shape. |
| Print-mode commands | Skills and slash commands expand in print mode; read-only commands return data without starting an agent turn. Use `--disable-slash-commands` to turn expansion off. |
| Markdown custom agents | Define agents in `agent.md` files with YAML frontmatter and Markdown instructions. |
| Enterprise authentication | Sign in with Gemini Enterprise, Workforce Identity Federation, or Application Default Credentials. |
| Safer permissions | Strict and request-review sessions no longer auto-approve commands, and empty allow rules match nothing. |
| Sandboxed Git metadata | The terminal sandbox grants read-only access to `.git`. |
| Plugin state | `config.json` is the single source for whether an installed plugin is enabled. |
| MCP and subagents | Long-running MCP tools report progress, and stopping a subagent also stops its descendants. |

## What changed in 1.1.12–1.1.27

The current release is `1.1.27`. These are the release changes most likely to affect everyday CLI use:

| Release | What it means |
|---|---|
| `1.1.12` | Print mode gained read-only slash commands and machine-readable `models`/`agents` output; press `t` to open the artifact outline. |
| `1.1.13` | Use `GEMINI_API_KEY` with `"modelProvider": "gemini"` for direct Gemini API access; code search has a local fallback. |
| `1.1.14–1.1.15` | Send persistent prompts through stdin with `--input-format stream-json`; Markdown agents can declare `rules:`, and plugins can provide `rules.json`. |
| `1.1.16` | Manage user-level MCP servers with `agy mcp add`, `list`, `enable`, `disable`, and `remove`. |
| `1.1.17–1.1.19` | Teamwork command handling improved, remote control accepts a free port, and `AGY_CLI_HIDE_LOGO` hides the startup banner. |
| `1.1.20–1.1.21` | Workspace reads work more smoothly under review mode. Voice input is available through `/voice`, `/record`, `F5`, and `mic-serve` over SSH. |
| `1.1.22–1.1.23` | `/model <name>` saves a model by name, slug, or label, and model completion works with `Tab`. |
| `1.1.24–1.1.25` | MCP files accept comments and trailing commas. `/resume` can group sessions by workspace, and Markdown agents inherit ambient customizations by default. |
| `1.1.26–1.1.27` | `Ctrl+D`/`Ctrl+U` scroll artifacts by half a page, `pickerGrouping` controls resume layout, and `/model <name> <prompt>` consults another model for one prompt. |

## Launch flags and headless mode

| Command or flag | Use |
|---|---|
| `agy -p "<prompt>"` | Run once and print the result. |
| `--input-format <format>` | Read a persistent prompt stream; use `stream-json` with matching output. |
| `--output-format <format>` | Return `text`, `json`, or `stream-json`. |
| `--json-schema <schema>` | Validate output against an inline schema or schema file. |
| `--disable-slash-commands` | Treat slash-prefixed print input as plain text. |
| `--model <slug>` | Select a model with a stable model slug. |
| `--effort <level>` | Select the model's reasoning-effort variant at launch. |
| `--mode <mode>` | Start in default, accept-edits, or plan mode. |
| `--agent <name>` | Start with a custom agent. |
| `agy models` | List models; add `--output-format json` for machine-readable output. |
| `agy agent`, `agy agents` | List available custom agents. |
| `--continue`, `-c` | Continue the most recent conversation. |
| `--conversation <id>` | Continue a specific conversation. |
| `--project <project>` | Open an existing project by name or ID. |
| `--new-project <name>` | Create a project. |
| `--sandbox` | Enable sandboxing for the session. |
| `--print-timeout <duration>` | Set the maximum print-mode wait. |
| `AGY_CLI_CMD_OUTPUT_PERCENTAGE` | Limit command output shown in the TUI. |
| `AGY_CLI_HIDE_LOGO` | Hide the startup logo. |
| `AGY_CLI_DISABLE_ESCAPE_SEQUENCE_OPTIMIZATIONS` | Disable terminal escape-sequence optimizations. |

Print mode supports structured usage, tool, and subagent data. `stream-json` emits NDJSON events as work progresses. Custom skills and slash commands expand by default; use `--disable-slash-commands` to turn that behavior off.

To keep one process open for several prompts, pair `--input-format stream-json` with `--output-format stream-json` and send one JSON user event per line. Close stdin when the session is complete; do not add `-p`. CLI-handled slash commands such as `/model` and `/usage` are unavailable in this stream-input mode.

Read-only commands such as `/usage`, `/quota`, `/credits`, `/model`, `/effort`, `/skills`, `/permissions`, `/hooks`, `/help`, `/changelog`, and `/config` return data without starting an agent turn. `agy models --output-format json` and `agy agents --output-format json` return machine-readable lists. Interactive-only commands fail with guidance. Tools that require approval are denied unless a matching allow rule exists.

## Authentication

Local sessions reuse valid credentials from Apple Keychain, Linux Secret Service/dbus, or Windows Credential Manager. If no saved session exists, `agy` opens the browser sign-in flow. SSH sessions print an authorization URL and accept the resulting code in the remote terminal.

Enterprise users can sign in with a Gemini Enterprise license on a Google Cloud project. The CLI also supports Workforce Identity Federation through advanced SSO and Application Default Credentials for Agent Platform access.

For direct Gemini API access, set the provider in `~/.gemini/antigravity-cli/settings.json`:

```json
{
  "modelProvider": "gemini"
}
```

Then set `GEMINI_API_KEY`:

```bash
export GEMINI_API_KEY="<your-api-key>"
# Optional custom Gemini endpoint
export GOOGLE_GEMINI_BASE_URL="<endpoint>"
```

On PowerShell, use `$env:GEMINI_API_KEY="<your-api-key>"`. Setting the environment variable without `"modelProvider": "gemini"` has no effect. The CLI does not read `.env` or `GOOGLE_API_KEY` for this mode, and `/logout` does not change API-key authentication.

## Slash commands

| Command | Use |
|---|---|
| `/add-dir <path>` | Add workspace folder. |
| `/agents` | Open subagent panel. |
| `/btw <query>` | Ask side question. |
| `/clear` (`/new`) | Clear terminal/context. |
| `/config` (`/settings`) | Open settings. |
| `/codesearch` (`/cs`, `/search`) | Search workspace code. |
| `/artifact` | Open artifact review. |
| `/context` | View context usage. |
| `/copy <n>` | Copy the latest or n-th most recent response. |
| `/credits` | View G1 credits. |
| `/diff` | Show file diffs. |
| `/exit` (`/quit`) | Close CLI. |
| `/fork` (`/branch`) | Fork session. |
| `/hooks` | View hooks. |
| `/help` | Open help. |
| `/keybindings` | Edit shortcuts. |
| `/logout` | Clear saved tokens. |
| `/mcp` | Manage MCP servers. |
| `/model` | Choose model. |
| `/model <name> <prompt>` | Use another model for one prompt, then return to the current model. |
| `/effort` (`/effort <level>`) | View or set reasoning effort. |
| `/open <path>` | Open file. |
| `/permissions` | Set approvals. |
| `/fast` | Enable fast mode. |
| `/planning` | Enter plan mode. |
| `/rename <name>` | Rename thread. |
| `/resume` (`/switch`, `/conversation`) | Resume session. |
| `/rewind` (`/undo`) | Roll back history. |
| `/skills` | Browse skills. |
| `/statusline` | Edit status bar. |
| `/tasks` | View shell logs. |
| `/title [on/off]` | Set terminal title. |
| `/voice` (`/record`) | Start voice dictation. |
| `/boost <task>` | Send a task to the boost workflow. |
| `/teamwork-preview <task>` (`/teamwork`) | Start a collaborative agent-team task. |
| `/usage` (`/quota`) | View model quota usage. |
| `/feedback` | Open feedback panel. |

### Voice input over SSH

Run the microphone service locally, forward it to the remote session, then set the remote endpoint:

```bash
# Local machine
agy mic-serve
ssh -R 24713:localhost:4713 user@remote-host

# Remote machine
export ANTIGRAVITY_MIC="localhost:24713"
agy
```

Use `/voice`, `/record`, or `F5` in the remote CLI. The transcript stays in the prompt until you submit it; `Esc` discards it. Keep `mic-serve` bound to localhost.

`/codesearch` uses regular expressions by default. Add `-F` or `--literal` for exact text. Use `f:` or `file:` globs to include or exclude paths. Results stream progressively, and `Esc` cancels an active search.

## Keyboard shortcuts

Use `/keybindings` to inspect or edit active shortcuts.

Vim editor mode is optional. Enable it under Editor Mode in `/settings`. In Normal mode, submit with `Ctrl+S` or `Ctrl+Enter`. Insert First starts prompts in Insert mode, where `Enter` submits and `Shift+Enter` or `Ctrl+J` inserts a newline.

| Shortcut | Action |
|---|---|
| `Enter` | Submit. |
| `Shift+Enter`, `Ctrl+J`, `Alt+Enter` | Newline. |
| `Shift+Tab` | Cycle execution modes. |
| `Esc` | Close, stop, or clear. |
| `Ctrl+C` | Cancel work; press twice to exit. |
| `Ctrl+D` | Forward-delete, exit on an empty prompt, or scroll an artifact half a page down. |
| `Ctrl+U` | Scroll an artifact half a page up. |
| `Ctrl+F` in `/resume` | Toggle workspace-grouped sessions. |
| `F5` | Start or stop voice dictation. |
| `Ctrl+L` | Clear screen. |
| `Ctrl+V` | Paste. |
| `Alt+V` | Alternative paste on Windows. |
| `Ctrl+G` | Open prompt, diff, or confirmation in `$EDITOR`. |
| `Ctrl+O` | Toggle details. |
| `Ctrl+R` | Review artifacts. |
| `F2`, `F4` | Rename or delete the selected item when those keybindings are active. |
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
| `copyOnSelect` | `true`; copy selected TUI text on mouse release. |
| `toolPermission` | `"request-review"`; approvals. |
| `artifactReviewPolicy` | `"asks-for-review"`; artifact review. |
| `notifications` | `false`; completion alerts. |
| `showTips` | `true`; prompt tips. |
| `showFeedbackSurvey` | `true`; feedback prompts. |
| `editor` | `"auto"`; external editor. |
| `editorMode` | `"default"`; flat or Vim prompt editing. |
| `pickerGrouping` | `"flat"`; set to `"grouped"` to group `/resume` sessions by workspace. |
| `vimInsertFirst` | `false`; start Vim prompts in Insert mode. |
| `allowNonWorkspaceAccess` | `false`; outside-root access; writes follow the current cycle mode. |
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

Add reusable grants under `permissions.allow` in `settings.json`. Permission resources include `command(git)`, `write_file(src/)`, `read_url(example.com)`, and `mcp(server/tool)`. Empty and comment-only command rules match nothing.

Recent releases allow workspace reads automatically in default review mode. Outside-workspace access still follows `allowNonWorkspaceAccess`, and writes use the current cycle mode once that access is enabled.

Enable the terminal sandbox:

```json
{
  "enableTerminalSandbox": true
}
```

## Subagents, plugins, skills, and MCP

Use `/agents` to inspect background subagents, including nested subagents. Use `/tasks` for shell logs. Use `/skills` for Agent Skills. Use `/mcp` for Model Context Protocol servers. Use `/hooks` for pre-flight and post-format hooks.

Use `--agent <name>` to select a custom agent at launch. The `agent` and `agents` subcommands list available agents. Custom agents use Markdown files with YAML frontmatter and Markdown instructions. Add `model` when a subagent should use a selected model tier; omit it to inherit the parent model. Current Markdown agents inherit ambient skills, rules, and subagents by default. Use `inheritCustomizations` when an agent needs explicit inheritance control, `rules:` to declare rule files, and `agents:` to declare dependent subagents.

| Scope | Custom agent path |
|---|---|
| Workspace | `.agents/agents/<name>.md` or `.agents/agents/<name>/agent.md` |
| Global | `~/.gemini/config/agents/<name>.md` or `~/.gemini/config/agents/<name>/agent.md` |

Plugin layout:

```text
~/.gemini/antigravity-cli/
|-- plugins/
|   `-- <plugin_name>/
|       |-- plugin.json
|       |-- mcp_config.json
|       |-- hooks.json
|       |-- rules.json
|       |-- skills/
|       |-- agents/
|       `-- rules/
`-- import_manifest.json
```

Installed plugin enablement is stored in `config.json`. The CLI discovers skills from both `skills.json` and the plugin's `skills/` directory.

Manage the global MCP profile from the CLI:

```text
agy mcp add <name> --type stdio --env KEY=value -- <command> [args]
agy mcp add <name> --type http --header "Header: value" <url>
agy mcp list
agy mcp enable <name>
agy mcp disable <name>
agy mcp remove <name>
```

These commands edit `~/.gemini/config/mcp_config.json`. Workspace servers remain in `.agents/mcp_config.json`. Recent releases accept comments and trailing commas in MCP config files.

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
- [Headless Mode](https://antigravity.google/docs/cli/headless/)
- [Voice Dictation](https://antigravity.google/docs/cli/commands/voice/)
- [MCP Configuration](https://antigravity.google/docs/mcp)
- [Antigravity CLI Releases](https://github.com/google-antigravity/antigravity-cli/releases)

## More ScriptByAI cheatsheets and resources

- [Claude Code Commands Cheat Sheet](https://www.scriptbyai.com/claude-code-commands-cheat-sheet/)
- [OpenAI Codex Commands Cheat Sheet](https://www.scriptbyai.com/codex-commands-cheat-sheet/)
- [The Ultimate Claude Code Resource List](https://www.scriptbyai.com/claude-code-resource-list/)
- [160+ Most Popular Agent Skills on GitHub for Coding Agents](https://www.scriptbyai.com/most-popular-agent-skills/)
