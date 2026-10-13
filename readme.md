# Antigravity CLI Cheatsheet

A compact reference for Google Antigravity CLI (`agy`): install commands, slash commands, shortcuts, settings, permissions, subagents, plugins, MCP, and Gemini CLI migration.

Updated for Antigravity CLI 1.3.4 on October 13, 2026.

## Contents

- [Install](#install)
- [Quick reference](#quick-reference)
- [Launch flags and headless mode](#launch-flags-and-headless-mode)
- [Authentication](#authentication)
- [Slash commands](#slash-commands)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Execution modes](#execution-modes)
- [Settings and paths](#settings-and-paths)
- [Permissions and sandbox](#permissions-and-sandbox)
- [Subagents, plugins, skills, and MCP](#subagents-plugins-skills-and-mcp)
- [Gemini CLI migration](#gemini-cli-migration)
- [Troubleshooting](#troubleshooting)
- [Official sources](#official-sources)

## Install

### macOS and Linux

```bash
curl -fsSL https://antigravity.google/cli/install.sh | bash
```

Binary: `~/.local/bin/agy`

### Windows PowerShell

```powershell
irm https://antigravity.google/cli/install.ps1 | iex
```

### Windows CMD

```cmd
curl -fsSL https://antigravity.google/cli/install.cmd -o install.cmd && install.cmd && del install.cmd
```

Windows binary: `C:\Users\<Username>\AppData\Local\agy\bin`

Native Android binaries are also available for direct use in Termux without proot-distro.

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
| Plan mode | `/plan <task>` or `Shift+Tab` |
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
| Remote Control | `--remote-control`, `/remote-control`, or `agy remote-control start/status/stop` |
| Subagent messaging | `@<subagent> <message>` |
| Task logs | `/tasks` |
| Skills | `/skills`, `/skills reload` |
| MCP manager | `/mcp`, `agy mcp` |
| Hooks | `/hooks` |
| Log out | `/logout` |
| Exit | `/exit`, `/quit` |

## Launch flags and headless mode

| Command or flag | Use |
|---|---|
| `agy -p "<prompt>"` | Run once and print the result. |
| `--input-format <format>` | Read a persistent prompt stream; use `stream-json` with matching output. |
| `--output-format <format>` | Return `text`, `json`, or `stream-json`. |
| `--json-schema <schema>` | Use an inline JSON schema or schema file with an object root; invalid schemas exit with code `1`. |
| `--disable-slash-commands` | Treat slash-prefixed print input as plain text. |
| `--model <slug>` | Select a model; base names use the default thinking level when effort is omitted. |
| `--effort <level>` | Select an effort supported by the chosen model. |
| `--dangerously-skip-permissions` | Skip tool permission prompts; artifact review still applies. |
| `--mode <mode>` | Start in default, accept-edits, or plan mode. |
| `--agent <name>` | Start with a custom agent. |
| `--remote-control` | Start a session-scoped Remote Control connection. |
| `agy models` | List models; add `--output-format json` for machine-readable output. |
| `agy agent`, `agy agents` | List available custom agents. |
| `--continue`, `-c` | Continue the most recent conversation. |
| `--conversation <id>` | Continue a specific conversation. |
| `--project <project>` | Open an existing project by name or ID. |
| `--new-project <name>` | Create a project. |
| `--sandbox` | Enable sandboxing for the session. |
| `--print-timeout <duration>` | Set a maximum print-mode wait; the default is unlimited. |
| `AGY_CLI_CMD_OUTPUT_PERCENTAGE` | Limit command output shown in the TUI. |
| `AGY_CLI_HIDE_LOGO` | Hide the startup logo. |
| `AGY_CLI_DISABLE_ESCAPE_SEQUENCE_OPTIMIZATIONS` | Disable terminal escape-sequence optimizations. |

Stream input requires matching `stream-json` output; read-only commands return data without an agent turn, and protected tools need matching allow rules. Print-mode model or API failures emit structured `AGY_ERROR` JSON on stderr and exit with code `3`.

Unknown `--agent` names exit with code `1` in headless mode; interactive sessions warn before using the default agent. Bare schema type names, missing schema files, and non-object roots exit with code `1`. Combining `--print` with `--prompt-interactive` exits with code `2`.

Remote Control is unavailable with Application Default Credentials (`AGY_ADC_AUTH`). Browser Settings exposes the CLI session settings; `agy remote-control serve` loads and saves `settings.json`.

## Authentication

Login: local sessions reuse Apple Keychain, Linux Secret Service/dbus, or Windows Credential Manager. SSH prints a browser URL and accepts the returned code. Enterprise sign-in supports Gemini Enterprise, Workforce Identity Federation, and Application Default Credentials.

API-key mode: set `"modelProvider": "gemini"` in `~/.gemini/antigravity-cli/settings.json`:

```json
{
  "modelProvider": "gemini"
}
```

```bash
export GEMINI_API_KEY="<your-api-key>"
# Optional custom Gemini endpoint
export GOOGLE_GEMINI_BASE_URL="<endpoint>"
```

PowerShell: `$env:GEMINI_API_KEY="<your-api-key>"`. The provider setting is required; `.env` and `GOOGLE_API_KEY` are not used for this mode. Exhausted daily quotas, project spend caps, or prepaid credits stop retries immediately; short per-minute limits still retry.

## Slash commands

| Command | Use |
|---|---|
| `/add-dir <path>` | Add workspace folder. |
| `/agents` | Open subagent panel. |
| `/btw <query>` | Ask a side question without running user or workspace hooks. |
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
| `/plugin` (`/plugins`) | Browse the marketplace or run inline `install`, `uninstall`, `enable`, `disable`, and `list`. |
| `/model` | Open the picker; search by model name or ID. |
| `/model <name>` | Switch model; base names use the default thinking level when effort is omitted. |
| `/model <name> <prompt>` | Use another model for one prompt, then return to the current model. |
| `/effort` (`/effort <level>`) | View or set an effort supported by the selected model. |
| `/open <path>` | Open file. |
| `/permissions` | Set approvals. |
| `/rename <name>` | Rename thread. |
| `/resume` (`/switch`, `/conversation`) | Resume session. |
| `/rewind` (`/undo`) | Roll back history. |
| `/skills` | Browse skills. |
| `/skills reload` | Reload discovered skills and slash commands without restarting. |
| `/statusline` | Edit status bar. |
| `/tasks` | Monitor background commands, timers, recurring jobs, and other background tools. |
| `/title [on/off]` | Set terminal title. |
| `/voice` (`/record`) | Start dictation; recordings finalize after 3 minutes 30 seconds. |
| `/boost <task>` | Run boost mode for a task. |
| `/teamwork-preview <task>` (`/teamwork`) | Start a collaborative agent-team task. |
| `/usage` (`/quota`) | View model quota usage. |
| `/feedback` | Open feedback panel. |
| `/remote-control` | Start Remote Control for this session; use `/remote-control off` to stop. |

`/codesearch` uses regular expressions by default. Add `-F` or `--literal` for exact text. Use `f:` or `file:` globs to include or exclude paths. Results stream progressively, and `Esc` cancels an active search.

## Keyboard shortcuts

Vim: set Editor Mode in `/settings`; Normal mode submits with `Ctrl+S` or `Ctrl+Enter`. Normal and Visual modes support numeric counts such as `3dw`, `2d3w`, and `3dd`. Edit bindings with `/keybindings`.

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
| `j` / `k` in diff file view | Move the line cursor down or up. |
| `←` / `→` in diff file view | Switch files. |
| `n` / `N` in diff file view | Next or previous change, including across files. |
| `Shift+Tab` in `/diff` | Switch to the previous view. |
| `←` / `→` in `/config` | Change and immediately save the highlighted setting. |
| `Esc` on an empty prompt with an announcement | Dismiss the current announcement permanently. |
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

Modes: choose Agent Mode in `/settings`, pass `--mode`, or press `Shift+Tab`.

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
| `verbosity` | Default: `"medium"`; values: `"high"`, `"medium"`, `"low"`; `"medium"` groups related tool calls and thoughts while keeping commands and responses visible. |
| `queuedMessages` | Queue follow-ups by default; `"send-immediately"` interrupts with new and queued messages in order. |
| `runningLightSpeed` | `"medium"`; progress animation. |

## Permissions and sandbox

| Preset | Behavior |
|---|---|
| `request-review` | Prompt before risky tools. |
| `proceed-in-sandbox` | Auto-proceed in sandbox. |
| `always-proceed` | No prompts. |
| `strict` | Prompt for all non-read tools. |

Allow rules live under `permissions.allow` in `settings.json`; resources include `command(git)`, `write_file(src/)`, `read_url(example.com)`, and `mcp(server/tool)`. Empty rules match nothing. Outside-workspace access follows `allowNonWorkspaceAccess`. Sandboxed commands can read and write the CLI artifact and scratch directories.

Windows sandboxed commands run without administrator rights. On macOS, sandboxed commands cannot read `~/Library/Application Support` by default; permit access explicitly in sandbox rules when required. Plans still wait for review with `--dangerously-skip-permissions`; set Artifact Review in `/config` to control automatic approval.

Enable the terminal sandbox:

```json
{
  "enableTerminalSandbox": true
}
```

## Subagents, plugins, skills, and MCP

Custom agents use Markdown files with YAML frontmatter. Launch with `--agent <name>`; use `agy agents` to list them. Frontmatter supports `model`, `rules:`, and `agents:`; ambient customizations are inherited by default. Message a running or completed subagent with `@<subagent> <message>`; autocomplete lists both.

| Scope | Custom agent path |
|---|---|
| Workspace | `.agents/agents/<name>.md` or `.agents/agents/<name>/agent.md` |
| Global | `~/.gemini/config/agents/<name>.md` or `~/.gemini/config/agents/<name>/agent.md` |

Manage plugins with `/plugin` or `/plugins`. Choose `+ Install from local directory...` for a local folder and complete required settings before enabling the plugin. In the Installed tab, type to search, press `Enter` to finish searching, then use `Space` to toggle a plugin or `Enter` to open details.

| Plugin command | Use |
|---|---|
| `agy plugin install <plugin-name>@antigravity-plugins-official` | Install from the official marketplace. |
| `agy plugin install <path>` | Install a local or remote package. |
| `agy plugin list` | List all installed plugins with version and enabled state. |
| `agy plugin list --json` | Return the installed plugin list as JSON. |
| `agy plugin enable <name>` / `agy plugin disable <name>` | Enable or disable a plugin. |
| `agy plugin uninstall <name>` | Remove a plugin. |

Plugin files: `plugin.json`, `mcp_config.json`, `hooks.json`, `rules.json`, `skills/`, `agents/`, `agents.json`, and `rules/` under `~/.gemini/antigravity-cli/plugins/<plugin_name>/`. Enablement is stored in `config.json`. Plugins placed directly in `~/.gemini/config/plugins/` that need MCP variables start disabled until enabled.

Plugins can provide executable tools in `bin/` and persistent storage through `PLUGIN_DATA`. If Windows blocks an update or removal because a plugin is open, close other `agy` sessions, Antigravity Hub, or the Remote Control daemon and retry.

Paths in custom-agent `skills:`, `plugins:`, `rules:`, `agents:`, and `hooks:` entries resolve relative to the agent file. Subagents with `branch` or `share` workspaces receive an isolated copy of each repository in a multi-repository workspace.

Directory entries in `skills.json`, `rules.json`, `agents.json`, and `plugins.json` load direct children only; use `include_only` for nested items. User and workspace rules share a 20,000-token budget. Customization manifests load from every parent `.agents/` directory up to the project root when a session starts in a subdirectory.

MCP CLI:

```text
agy mcp add <name> --type stdio --env KEY=value -- <command> [args]
agy mcp add <name> --type http --header "Header: value" <url>
agy mcp list
agy mcp enable <name>
agy mcp disable <name>
agy mcp remove <name>
```

User profile: `~/.gemini/config/mcp_config.json`. Workspace profile: `.agents/mcp_config.json`. Comments and trailing commas are supported. MCP servers can keep a tool loaded with `_meta: {"antigravity/alwaysLoad": true}` in its `tools/list` entry. Temporary network failures do not clear saved OAuth sign-ins.

## Gemini CLI migration

First launch: `agy` detects legacy Gemini CLI profiles. Manual extension import: `agy plugin import gemini`.

Context files:

```text
GEMINI.md
AGENTS.md
~/.gemini/GEMINI.md
```

Skills:

| Scope | Path change |
|---|---|
| Global skills | `~/.gemini/skills/` to `~/.gemini/antigravity-cli/skills/` |
| Workspace skills | `.gemini/skills/` to `.agents/skills/` |

MCP config:

| Gemini CLI | Antigravity CLI |
|---|---|
| `~/.gemini/settings.json` | `~/.gemini/config/mcp_config.json` |
| Inline MCP server definitions | Standalone `mcp_config.json` profiles |
| `url` or `httpUrl` | `serverUrl` |

## Troubleshooting

| Problem | Action |
|---|---|
| `Authentication expired. Please log back in.` | Sign in again. |
| Unknown custom agent | Run `agy agents` and use a listed name. |
| Invalid headless schema | Use a JSON schema with `"type": "object"` at the root. |
| Remote Control with ADC fails | Use a supported sign-in method; ADC is unavailable for Remote Control. |

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
