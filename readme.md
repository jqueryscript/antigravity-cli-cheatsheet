# Antigravity CLI Cheatsheet

A compact reference for Google Antigravity CLI (`agy`): install commands, slash commands, shortcuts, settings, permissions, subagents, plugins, MCP, and Gemini CLI migration.

Updated for Antigravity CLI 1.5.8 on November 13, 2026.

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
| Free conversation context | `/compact` |
| Rewind session | `/rewind` |
| Fork session | `/fork` |
| Add directory | `/add-dir <path>` |
| `/projects` | List, create, edit, or switch projects. Switching local projects adds their folders and starts a new conversation. |
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
| `--dangerously-skip-permissions` | Skip routine tool prompts. Explicit denies, the system command denylist, enterprise policy, and artifact review still apply. |
| `--mode <mode>` | Start in default, accept-edits, auto (where available), or plan mode. |
| `--agent <name>` | Start with a custom agent. |
| `--remote-control` | Start a session-scoped Remote Control connection. |
| `--remote-agent` | Run on a remote agent. Use `--project <name-or-id>` to choose a remote project, or omit it to use your default. |
| `agy acp` | Start an Agent Client Protocol server over stdio for an editor. Use `agy acp --login` to sign in from a terminal. |
| `agy models` | List models; add `--output-format json` for machine-readable output. |
| `agy agent`, `agy agents` | List available custom agents. |
| `--continue`, `-c` | Continue the most recent conversation. |
| `--conversation <id>` | Continue a specific conversation. |
| `--project <project>` | Open an existing project by name or ID. |
| `--new-project <name>` | Create a project. |
| `--sandbox` | Enable sandboxing for the session. |
| `--print-timeout <duration>` | Set a maximum headless wait. A timed-out turn returns available partial output and exits with code `3`. |
| `AGY_CLI_CMD_OUTPUT_PERCENTAGE` | Limit command output shown in the TUI. |
| `AGY_CLI_HIDE_LOGO` | Hide the startup logo. |
| `AGY_CLI_OSC7501` | `off` disables terminal status reporting; `on` forces it for a supporting terminal. |
| `AGY_CLI_OSC7501_MESSAGES` | `brief` omits pending command and question details from terminal status reports. |
| `AGY_CLI_DISABLE_ESCAPE_SEQUENCE_OPTIMIZATIONS` | Disable terminal escape-sequence optimizations. |

Stream input requires matching `stream-json` output; read-only commands return data without an agent turn, and protected tools need matching allow rules. Print-mode model or API failures emit structured `AGY_ERROR` JSON on stderr and exit with code `3`.

Unknown `--agent` names exit with code `1` in headless mode; interactive sessions warn before using the default agent. Bare schema type names, missing schema files, and non-object roots exit with code `1`. Combining `--print` with `--prompt-interactive` exits with code `2`.

Remote Control is unavailable with Application Default Credentials (`AGY_ADC_AUTH`). Browser Settings exposes the CLI session settings; `agy remote-control serve` loads and saves `settings.json`.

Without `--print-timeout`, a headless run waits for background tasks. JSON and stream-JSON results include `finished_reason` on early exit. Check for `print_timeout`, `interrupted`, or `error` before treating a result as complete.

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

With `GEMINI_API_KEY` or Application Default Credentials (`AGY_ADC_AUTH`), `/model` and `--model` offer Gemini 4 Hydrogen (Low, Medium, High, XHigh) and Gemini 4 Carbon (Low, Medium, High). Run `agy models` to find the available model slugs. Google Cloud sign-in lets you choose a Gemini subscription or Pay As You Go.

## Slash commands

| Command | Use |
|---|---|
| `/add-dir <path>` | Add workspace folder. |
| `/agents` | Open subagent panel. |
| `/btw <query>` | Ask a side question. Read and search tools can run, and calls that require approval stop the side question. |
| `/clear` (`/new`) | Clear terminal/context. |
| `/config` (`/settings`) | Open settings. |
| `/codesearch` (`/cs`, `/search`) | Search workspace code. |
| `/artifact` | Open artifact review. |
| `/context` | View context usage. |
| `/compact`, `/compact <message>` | Summarize the conversation; add a message to continue afterward. |
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
| `/schedule <when> <task>` | Schedule a one-time or recurring task. |
| `/title [on/off]` | Set terminal title. |
| `/voice` (`/record`) | Start dictation; recordings finalize after 3 minutes 30 seconds. |
| `/boost <task>` | Run boost mode for a task. |
| `/teamwork-preview <task>` (`/teamwork`) | Start a collaborative agent-team task. |
| `/swarm <task>` | Start a preview swarm with parallel workers and a shared message board. Each worker uses your quota. |
| `/swarm board` | Open the live roster and posts. `Alt+S` opens the board and `Esc` goes back. |
| `/usage` (`/quota`) | View model quota usage. |
| `/feedback` | Open feedback panel. |
| `/remote-control` | Start Remote Control for this session; use `/remote-control off` to stop. |

`/codesearch` uses regular expressions by default. Add `-F` or `--literal` for exact text. Use `f:` or `file:` globs to include or exclude paths. Results stream progressively, and `Esc` cancels an active search.

`/compact` summarizes the conversation and ends the turn; `/compact <message>` compacts first, then answers the message. Attachments remain available after compaction. Press `Esc` to cancel while it runs.

Recurring cron schedules may use a fixed offset within their period; request exact timing when needed. Fixed-time daily jobs stay within their scheduled minute; `@every` schedules are unchanged.

Remote sessions search remote files for `@` mentions. Cloud plugin skills connected through `/plugin` appear in the slash menu and under Remote agent skills in `/skills`. In `/config`, the TUI tab configures the terminal interface and the Remote Agent tab configures the remote agent.

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
| `Alt+M` | Send immediately in Queue mode, or queue in Send Immediately mode. |
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
| `n` / `N` in diff file view | Next or previous change across files; during search, step through matches. |
| `Shift+Tab` in `/diff` | Switch to the previous view. |
| `/` in `/diff` | Search the file list or file view; `Enter` jumps to the first match, `n` / `N` steps through matches. |
| `Ctrl+O` / `f` in diff file view | Toggle the nearest fold / whole-file view in All Changes, Per Turn, and Commit Tree. |
| `[` / `]` in Per Turn | Jump to the first file of the previous or next turn in the list or file view; wrap at either end. |
| `←` / `→` in `/config` | Change and immediately save the highlighted setting. |
| `Esc` on an empty prompt with an announcement | Dismiss the current announcement permanently. |
| `Alt+J` | Jump to subagent. |
| `Ctrl+K` | Approve a pending subagent action or open its question picker. |
| `Ctrl+A` | Cursor start. |
| `Ctrl+E` | Cursor end. |
| `Ctrl+_`, `Ctrl+Shift+-` | Undo text; `Ctrl+Z` also works where supported. |
| `Ctrl+Shift+Z` | Redo text. |
| `Up` / `Down` | Move selection. |
| `PgUp` / `Shift+Up` | Scroll up. |
| `PgDn` / `Shift+Down` | Scroll down. |
| `Tab` | Accept autofill. |
| `y` / `n` at confirmation | Approve or reject. |
| `A` | Approve all artifacts. |

Paste an image with the terminal paste shortcut, such as `Cmd+V` or `Ctrl+Shift+V`, or drag an image file into the prompt. SSH image paste supports X11 or Wayland forwarding; install `wl-clipboard` if the CLI reports it missing on Wayland.

Diff folding and whole-file view require the file on disk to match the diff and are disabled above 100,000 lines or 10 MB. Binary files and untracked files over 1 MB appear in the file list without a text preview.

macOS Option shortcuts work by default. Set `AGY_CLI_MAC_OPTION_AS_ALT=0` to restore the previous behavior. In `full screen (altscreen)` rendering mode, click a tool group, thought, image attachment, or multiline command output to expand or collapse it.

## Execution modes

Modes: choose Agent Mode in `/settings`, pass `--mode`, or press `Shift+Tab`.

| Mode | Behavior |
|---|---|
| `default` | Review file writes before they run. |
| `accept-edits` | Accept file edits automatically. |
| `auto` (Preview) | Approve workspace edits and sandboxed commands automatically; review commands outside the sandbox and prompt for risky actions. |
| `plan` | Plan without applying edits. |

With no saved Agent Mode, interactive and headless sessions start in Auto where available and Default elsewhere. To always start in Default, choose Default under Agent Mode in `/config`. This saves `"agentMode": ""` in `settings.json`. The `strict` tool permission preset still requires approval for edits in Auto and Accept Edits.

Auto mode requires a supported terminal sandbox and an eligible account, such as a personal Google Account or a pay-as-you-go Google Cloud account using Application Default Credentials. Gemini Enterprise follows its admin permission policy. On Windows, Auto requires Windows 11 24H2 or later with the August 2026 update or later. An explicit headless `--mode=auto` falls back to Default with a warning if Auto is unavailable.

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
| `agentMode` | `""` (Default), `"accept-edits"`, `"auto"`, or `"plan"`. An unset value selects Auto where available. |
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
| `pickerGrouping` | `"none"`; set to `"grouped"` to group `/resume` sessions by workspace. |
| `vimInsertFirst` | `false`; start Vim prompts in Insert mode. |
| `allowNonWorkspaceAccess` | `false`; outside-root access; writes follow the current cycle mode. |
| `enableTerminalSandbox` | `false`; command sandboxing. |
| Enable Telemetry in `/config` | Account preference; no `settings.json` key. Business accounts show `off (disabled)`. |
| `verbosity` | Default: `"medium"`; values: `"high"`, `"medium"`, `"low"`; `"medium"` groups related tool calls and thoughts while keeping commands and responses visible. |
| `queuedMessages` | Queue follow-ups by default; `"send-immediately"` interrupts with new and queued messages in order. |
| `runningLightSpeed` | `"medium"`; progress animation. |

## Permissions and sandbox

| Preset | Behavior |
|---|---|
| `request-review` | Prompt before risky tools. |
| `proceed-in-sandbox` | Auto-run sandboxed commands; fall back to `request-review` if the sandbox is unavailable. |
| `always-proceed` | No prompts unless an ask or deny rule matches. |
| `strict` | Prompt before every tool action, including reads; allow rules do not bypass it. |

Allow rules live under `permissions.allow` in `settings.json`; resources include `command(git)`, `write_file(src/)`, `read_url(example.com)`, and `mcp(server/tool)`. Empty rules match nothing. Outside-workspace access follows `allowNonWorkspaceAccess`. Sandboxed commands can read and write the CLI artifact and scratch directories.

Windows sandboxed commands run without administrator rights on supported Windows 11 systems. If the sandbox is unavailable, sandboxed permission presets fall back to review prompts; `--sandbox` and `enableTerminalSandbox` have no effect. On macOS, sandboxed commands cannot read `~/Library/Application Support` by default; permit access explicitly in sandbox rules when required. Sandboxed commands cannot read or modify `~/.docker`, `~/.kube`, or `~/_netrc` without explicit access. Plans still wait for review with `--dangerously-skip-permissions`; set Artifact Review in `/config` to control automatic approval.

Conversation-level approvals apply across the main agent and its subagents. Approving `execute_url` for a Google site does not also approve sensitive Google apps such as `mail.google.com`, `docs.google.com`, `drive.google.com`, `accounts.google.com`, or `console.cloud.google.com`; each needs its own approval.

Enable the terminal sandbox:

```json
{
  "enableTerminalSandbox": true
}
```

Always-allow command rules require an exact match in Windows PowerShell and Command Prompt. MCP ask and deny rules also apply to eager servers and tools used by the `/browser` subagent. Enterprise-required reviews take precedence over user allow rules and hook auto-approval. In headless runs, `--dangerously-skip-permissions` denies actions that require enterprise review.

Enterprise model gateways require HTTPS for remote hosts. Admins can set `gateway.routeWebFetch` to require `proxy.address` for URL reads and use `featureControls.remoteControlEnabled` to disable Remote Control.

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
| `agy plugin validate <path> [--strict] [--json]` | Validate a plugin directory or `plugin.json`; `--strict` fails on warnings, `--json` returns a CI report. |
| `agy plugin enable <name>` / `agy plugin disable <name>` | Enable or disable a plugin. |
| `agy plugin uninstall <name>` | Remove a plugin. |

Open `Configure MCP servers...` in an installed plugin to set or change server variables. Saving restarts its enabled servers; `needs setup` marks missing values.

Use `agy plugin validate <path> --strict` before publishing. It checks manifests, skills, agents, rules, hooks, sidecars, MCP configuration, context providers, and referenced files. Use lowercase kebab-case plugin names and valid logo paths. Hook stdin JSON includes `lastUserInput` with the latest user message.

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

User profile: `~/.gemini/config/mcp_config.json`. Workspace profile: `.agents/mcp_config.json`. Comments and trailing commas are supported. MCP servers can keep a tool loaded with `_meta: {"antigravity/alwaysLoad": true}` in its `tools/list` entry. Temporary network failures do not clear saved OAuth sign-ins. Plugin MCP OAuth tokens belong to the signed-in user; changing accounts or signing out reconnects the servers and may require another sign-in. MCP `tools/call` requests include `_meta["antigravity.google/root_conversation_id"]` with the top-level conversation ID.

Hook input includes `hookEventName` and `cwd`. Use `toolCall.id` to pair pre- and post-tool calls and `continuation_active` in a Stop hook to avoid requesting the same continuation twice. A PreInvocation hook can return `"terminationBehavior": "terminate"` to stop the turn before it reaches the model. Plugin hooks receive `PLUGIN_ROOT` and `PLUGIN_DATA`. Relative paths in hook commands support older CLI versions.

Plugin API key providers in `auth.json` can use `"optional": true`. A missing key then allows unauthenticated requests. When importing Claude Code plugins, check the summary for hooks or MCP servers that need manual migration. Workspace plugin and skill entries must resolve inside the workspace or repository root.

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
| Auto mode is missing | Check account eligibility and terminal sandbox support. Windows needs Windows 11 24H2 or later with the August 2026 update or later. |
| Plugin or plugin MCP server needs sign-in | Open `/plugin` and connect the plugin from the Installed tab. |
| Endpoint security blocks bundled ripgrep | Set `ANTIGRAVITY_DISABLE_EMBEDDED_RIPGREP=true` to use ripgrep on your PATH or the built-in search. |
| Docker or Kubernetes credentials are blocked | Grant sandbox access explicitly to the required `~/.docker` or `~/.kube` path. |
| Image paste finds no image | Copy only an image, then use the terminal paste shortcut or drag the image file into the prompt. Install the clipboard utility named in the error. |

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
