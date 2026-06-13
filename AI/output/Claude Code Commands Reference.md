
## Part 1 — CLI Commands (Terminal)

These are invoked from your shell before or instead of an interactive session.

### Session Control

| Command | Description |
|---------|-------------|
| `claude` | Start interactive session |
| `claude "query"` | Start interactive session with initial prompt |
| `claude -p "query"` / `--print` | Execute query and exit (non-interactive, for scripting/automation) |
| `claude -c` / `--continue` | Load and continue most recent conversation in current directory |
| `claude -r "<session>"` / `--resume` | Resume specific past session by ID or name |
| `claude -w "<name>"` / `--worktree` | Start in isolated git worktree (prevents touching main branch) |
| `claude --bare` | Minimal mode: skip hooks, skills, plugins, MCP, auto memory, CLAUDE.md. Up to 10x faster startup |
| `claude --remote "task"` | Create a new web session on claude.ai with the given task |
| `claude --remote-control` / `--rc` | Start interactive session with Remote Control enabled (controllable from claude.ai or mobile app) |
| `claude remote-control` | Start a dedicated Remote Control server (server mode, no local interactive session) |
| `claude --teleport` | Resume a web session in your local terminal |
| `claude --bg "<prompt>"` | Launch as background daemon process via tmux |

### Account & Authentication

| Command | Description |
|---------|-------------|
| `claude auth login` | Sign in to Anthropic account. Use `--console` for API billing instead of subscription |
| `claude auth logout` | Sign out |
| `claude auth status` | Show auth status as JSON. `--text` for human-readable. Exit code 0=logged in, 1=not |

### Management & Configuration

| Command | Description |
|---------|-------------|
| `claude update` | Update CLI to latest version |
| `claude agents` | List all configured subagents, grouped by source |
| `claude mcp` | Configure MCP (Model Context Protocol) servers |
| `claude plugin` / `claude plugins` | Manage plugins (install, uninstall, list) |
| `claude auto-mode defaults` | Print built-in auto mode classifier rules as JSON |
| `claude auto-mode config` | Show effective auto mode configuration with all settings applied |

### Key CLI Flags

| Flag | Description |
|------|-------------|
| `--model <name>` | Set model for session (e.g. `claude-sonnet-4-6`, alias `sonnet` or `opus`) |
| `--permission-mode <mode>` | Start in a permission mode: `default`, `acceptEdits`, `plan`, `auto`, `dontAsk`, `bypassPermissions` |
| `--dangerously-skip-permissions` | Skip all permission prompts (equivalent to `bypassPermissions`) |
| `--effort <level>` | Set effort: `low`, `medium`, `high`, `max` (Opus 4.6 only) |
| `--max-turns <n>` | Limit agentic turns in print mode |
| `--max-budget-usd <n>` | Max USD to spend on API calls before stopping (print mode) |
| `--system-prompt "text"` | Replace entire system prompt |
| `--append-system-prompt "text"` | Append to default system prompt |
| `--system-prompt-file <path>` | Replace system prompt with file contents |
| `--append-system-prompt-file <path>` | Append file contents to default system prompt |
| `--add-dir <path>` | Add additional working directories for file access |
| `--allowedTools "Tool(pattern)"` | Tools that execute without permission prompts |
| `--disallowedTools "Tool(pattern)"` | Tools removed from model context entirely |
| `--tools "Bash,Edit,Read"` | Restrict which built-in tools Claude can use |
| `-n` / `--name "<name>"` | Set display name for session |
| `--verbose` | Enable verbose logging (full turn-by-turn output) |
| `-v` / `--version` | Output version number |
| `--chrome` | Enable Chrome browser integration |
| `--agent-teams` | Enable experimental agent teams |
| `--bare` | Skip all auto-discovery for fast scripted calls |
| `--debug "categories"` | Enable debug mode with optional category filter |
| `--debug-file <path>` | Write debug logs to file |
| `--mcp-config <path>` | Load MCP servers from JSON file |
| `--strict-mcp-config` | Only use MCP servers from `--mcp-config` |
| `--output-format <fmt>` | Output format for print mode: `text`, `json`, `stream-json` |
| `--tmux` | Create tmux session for worktree (requires `--worktree`) |
| `--fork-session` | Create new session ID when resuming instead of reusing original |
| `--from-pr <number>` | Resume sessions linked to a GitHub PR |
| `--enable-auto-mode` | Unlock auto mode in Shift+Tab cycle (requires Team/Enterprise/API plan) |

---

## Part 2 — Interactive Slash Commands (Inside a Session)

These are typed at the Claude Code prompt during an active session.

### Confirmed Built-in Commands

| Command | Description |
|---------|-------------|
| `/clear` | Clear all context in the current session. Use to start a fresh task and avoid context contamination |
| `/compact` | Compress conversation history into a dense summary, freeing context space |
| `/cost` | Show token cost (USD) for the current session |
| `/init` | Scan current codebase, detect tech stack, generate `CLAUDE.md` behavior contract |
| `/memory` | Manage persistent memory rules; view loaded `CLAUDE.md` / `.claude/rules/` files |
| `/model` | Switch the model for the current session |
| `/config` | Open interactive configuration UI to view/modify global and project settings |
| `/agents` | List, create, edit, or delete custom subagents |
| `/hooks` | View configured lifecycle hooks, event matchers, and their sources (read-only) |
| `/debug` | Access session debug logs or stream CLI runtime debug info |
| `/rename` | Rename the current session (shown in `/resume` list and terminal title) |
| `/plan` | Enter plan mode — Claude outputs an architecture plan before writing code. Also toggled with `Shift+Tab` |

### Commands in This Installation (User-Defined Skills, Not Built-in)

The following appear as `/commands` in this Claude Code instance but are **skills** (user-defined), not official built-in commands. They may not exist in other installations.

| Command | What it does here |
|---------|-------------------|
| `/loop <interval> <command>` | Run a command on a recurring interval (e.g. `/loop 5m npm test`) |
| `/simplify` | Review changed code for quality/efficiency and apply fixes |
| `/code-review` | Review current diff for bugs and cleanups |
| `/verify` | Run the app and observe behavior to confirm a change works |
| `/gitpush` | Stage, commit, and push changes |
| `/run` | Launch the project app |
| `/statusline` | Configure the terminal status line display |

---

## Part 3 — Recommended Workflows

These are based on confirmed, real commands.

### Starting a new project module
```
# 1. Generate behavior contract
/init

# 2. Switch to plan mode before writing code
/plan
# or press Shift+Tab to toggle plan mode

# 3. Check cost periodically during long sessions
/cost

# 4. Compress context when it grows large
/compact
```

### Parallel development with worktrees
```bash
# Terminal 1: feature branch
claude -w feature-auth

# Terminal 2: separate feature
claude -w feature-payments
```

### Non-interactive scripting
```bash
# Pipe file content and get a response
cat logs.txt | claude -p "summarize the errors"

# JSON output for programmatic use
claude -p "list all TODO comments" --output-format json

# Cap spend for automated tasks
claude -p --max-budget-usd 2.00 --max-turns 10 "refactor this file"
```

### Remote/mobile session handoff
```bash
# Start session with remote control enabled
claude --remote-control "my-feature"

# On another machine: teleport a web session to local terminal
claude --teleport
```

---

## Sources

- Official CLI reference: https://code.claude.com/docs/en/cli-reference
- Claude Code built-in commands: https://code.claude.com/docs/en/commands
- Permission modes: https://code.claude.com/docs/en/permission-modes
- Remote Control: https://code.claude.com/docs/en/remote-control
- Worktrees: https://code.claude.com/docs/en/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees
