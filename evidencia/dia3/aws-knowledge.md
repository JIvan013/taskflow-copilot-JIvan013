# Copilot CLI Session

> [!NOTE]
> - **Session ID:** `f813f107-73b9-448e-9995-c0411bbf3997`  
> - **Started:** 9/16/2026, 12:41:56 PM  
> - **Duration:** 8m 36s  
> - **Exported:** 9/16/2026, 12:50:32 PM  

---

<sub>2s</sub>

### Info

GitHub CLI (gh) is not installed. Install it with: winget install --id GitHub.cli

---

<sub>2s</sub>

### Info

Tip: /instructions

---

<sub>6s</sub>

### User

copilot

---

<sub>15s</sub>

### Copilot

Voy a leer la documentación oficial del Copilot CLI para dar una respuesta precisa sobre sus capacidades y comandos. Ejecutando fetch_copilot_cli_documentation.

---

<sub>15s</sub>

### `fetch_copilot_cli_documentation`

<details>
<summary>Arguments</summary>

```json
{}
```

</details>

<details>
<summary>131 lines</summary>

```
# GitHub Copilot CLI Documentation

## README
[Could not load README: Error: ENOENT: no such file or directory, open 'C:\Users\admin\AppData\Local\copilot\pkg\win32-x64\1.0.85\README.md']

## Help Command Output
Global
  /help - show full help
  ? - show quick help
  / - commands
  @ - mention files
  # - mention issues and pull requests
  ! - execute shell command
  shift+tab - switch modes
  ctrl+s - stash/pop current prompt
  ctrl+q - enqueue prompt
  ctrl+r - reverse search history
  ctrl+o - toggle all timeline
  ctrl+c - cancel
  ctrl+c×2 - exit
  esc esc - clear input, interrupt, stop agents, or rewind
  ctrl+d - shutdown
  ctrl+z - suspend
  ctrl+l - clear screen
  ctrl+t - toggle reasoning display
  ctrl+x → b - move current task to background
  ctrl+x → g - collapse or expand the autopilot goal panel
  ctrl+x → o - open most recent link

Input
  ctrl+a - go to line start
  ctrl+e - go to line end
  ctrl+h - delete previous character
  ctrl+w - delete previous word
  ctrl+u - delete from cursor to beginning of line
  ctrl+k - delete from cursor to end of line
  meta+←/→ - move cursor by word
  shift+enter - insert newline
  ctrl+g - edit prompt in $EDITOR

Agent Environment
  /init - Initialize Copilot instructions for this repository
  /agent - Browse and select agents: /agent [name]
  /skills - Manage skills for enhanced capabilities
  /mcp - Manage MCP server configuration
  /plugin - Manage plugins and plugin marketplaces

Agents / Subagents
  /model - Select the AI model for this session (use 'auto' to let Copilot pick automatically). Use /config model to set the user default, '--repo'/'--local' to set the repo default, or 'plan'/'--plan' to set the plan-mode model.
  /delegate - Send this session to GitHub and Copilot will create a PR; use --base to choose the PR target branch
  /fleet - Enable fleet mode for parallel subagent execution
  /autopilot - Toggle autopilot mode, or set an autopilot objective with an optional AI-credit limit (--max-ai-credits)
  /tasks - View and manage tasks (subagents and shell commands)

Code
  /ide - Connect to an IDE workspace
  /diff - Review the changes made in the current directory
  /pr - Operate on pull requests for the current branch
  /review - Run code review agent to analyze changes
  /security-review - Analyze staged and unstaged changes for security vulnerabilities.
  /rubber-duck - Get an independent critique of your current work from the rubber duck agent
  /lsp - Manage language server configuration
  /terminal-setup - Configure terminal for multiline input support (shift+enter)

Permissions
  /permissions - Switch between permission modes
  /allow-all - Enable all permissions (tools, paths, and URLs)
  /add-dir - Allow file access to a directory and load its .github skills and agents as trusted configuration
  /list-dirs - Display all allowed directories for file access
  /cwd - Change working directory or show current directory
  /reset-allowed-tools - Reset the list of allowed tools

Session
  /resume - Switch to a different session (optionally specify session ID, task ID, or name)
  /rename - Rename the current session, or auto-generate a name from conversation
  /fork - Fork the current session into a new session, optionally with a name
  /worktree - Create a new git worktree from your configured base ref and switch into it, leaving your uncommitted changes behind. Use `/worktree new [prompt]` to start a separate conversation in the new worktree; `new` is reserved and cannot be used as a literal branch name.
  /move - Move your uncommitted changes into a new git worktree and switch into it. Pass a branch name, a task to start, or nothing to auto-name.
  /context - Show context window token usage and visualization
  /usage - Display session usage metrics and statistics
  /session - View and manage sessions. Use subcommands for details.
  /compact - Summarize conversation history to reduce context window usage. Optionally provide focus instructions.
  /share - Share session or research report to a markdown file, HTML file, GitHub gist, or a shareable GitHub link
  /remote - Share this session: remote control from GitHub web and mobile, or directly with another CLI
  /copy - Copy the last response to the clipboard
  /rewind - Rewind the last turn and revert file changes

Help
  /help - Show help for interactive commands
  /changelog - Display changelog for CLI versions. Add 'summarize' to get an AI summary.
  /feedback - Provide feedback about the CLI
  /diagnose - Analyze the current session log, optionally with a custom prompt
  /theme - View or set color mode
  /statusline - Configure status line items
  /footer - Configure status line items
  /update - Update the CLI to the latest version
  /version - Display version information and check for updates
  /experimental - Show available experimental features, or enable/disable experimental mode
  /memory - Show memory status, or enable/disable memory across sessions
  /clear - Abandon this session and start fresh
  /instructions - View and toggle custom instruction files
  /app - Prefer a visual workspace? Try out the GitHub Copilot desktop app

Other commands
  /ask - Ask a quick side question without adding to conversation history
  /chronicle - Session history tools and insights
  /collect-debug-logs - Collect debug logs to a local .tgz file or secret GitHub gist
  /computer - Show or toggle Computer Use
  /env - Show loaded environment details (instructions, MCP servers, skills, agents, hooks, plugins, LSPs, extensions)
  /exit - Exit the CLI; use 'print' to print the session after exiting alt screen
  /keep-alive - Manage keep-alive mode (prevents system sleep).
  /limits - View or edit session limits; the AI Credit limit is a soft cap
  /login - Log in to Copilot
  /logout - Log out of an OAuth login session
  /new - Start a new conversation
  /plan - Create an implementation plan before coding
  /refine - Rewrite a rough, stream-of-consciousness prompt into a clear one for review (Ctrl+X / then /refine cleans up your current input)
  /research - Run deep research investigation using GitHub search and web sources
  /restart - Restart the CLI, restoring supported live sessions in this process
  /search - Search the conversation timeline
  /settings - Open the settings UI, show, set, or unset a single value ('/settings unset <key>' removes it). Use '--repo'/'--local' to target repo settings.
  /subagents - Configure default and per-agent subagent models
  /user - Manage GitHub user list
  /vim - Toggle Vim mode for the input (hjkl/w/b/e/ge/0/^/$/gg/G motions, f/F/t/T/;/, char search, i/a/o insert, r/~/J/x/D/C edit, d/c/y operators, y/p/P yank & put, . repeat, u/ctrl+r undo & redo, counts, esc for normal mode)
  /voice - Manage voice mode (dictation transcription via Foundry Local)

Copilot respects instructions from these locations:
  CLAUDE.md (in git root & cwd)
  GEMINI.md (in git root & cwd)
  AGENTS.md (in git root & cwd)
  .github/instructions/**/*.instructions.md (in git root & cwd)
  .github/copilot-instructions.md (in git root & cwd)
  $HOME/.copilot/copilot-instructions.md
  $HOME/.copilot/instructions/**/*.instructions.md
  COPILOT_CUSTOM_INSTRUCTIONS_DIRS (additional directories via env var)

To learn about what I can do
  Ask me: "What can you do?"
  Or visit: https://docs.github.com/copilot/how-tos/use-copilot-agents/use-copilot-cli

---

**Note**: If the above documentation doesn't answer the user's request, you can fetch additional documentation from the official GitHub Docs for more information.

Use the web_fetch tool to fetch https://docs.github.com/api/article/body?pathname=/en/copilot/how-tos/use-copilot-agents/use-copilot-cli

You can also follow hyperlinks to related docs pages using the same API pattern.
```

</details>


---

<sub>15s</sub>

### User

Usa solo el servidor MCP aws-knowledge, sin la web ni la terminal. ¿Amazon DynamoDB y AWS CodeDeploy están disponibles en la región us-east-2? Responde en tres líneas y di qué herramienta usaste.

---

<sub>57s</sub>

### Copilot

Consultando la disponibilidad regional en el servidor AWS knowledge para los productos solicitados en us-east-2. Ejecutando aws-knowledge-aws___get_regional_availability.

---

<sub>59s</sub>

### `aws-knowledge-aws___get_regional_availability`

<details>
<summary>Arguments</summary>

```json
{
  "resource_type": "product",
  "filters": [
    "Amazon DynamoDB",
    "AWS CodeDeploy"
  ],
  "regions": [
    "us-east-2"
  ]
}
```

</details>

```
{"content":{"result":{"products":{"Amazon DynamoDB":{"status":"isAvailableIn"},"AWS CodeDeploy":{"status":"isAvailableIn"}},"next_token":null,"failed_regions":null}}}
```


---

<sub>1m 1s</sub>

### Copilot

Sí — Amazon DynamoDB está disponible en us-east-2 y AWS CodeDeploy también.  
Fuente: aws-knowledge-aws___get_regional_availability (servidor MCP aws-knowledge).

---

<sub>Generated by [GitHub Copilot CLI](https://github.com/features/copilot/cli)</sub>
