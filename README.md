### Cursor Commands: How to Run

These are custom Cursor commands that you can run directly from Cursor Chat using the `/` menu. Include this `.cursor/commands` folder in your repo to share the commands on GitHub; anyone who opens the repo in Cursor will see and can run them.

References: see Cursor Docs for Commands and Slash Commands and MCP/Tools configuration:
- [Cursor Docs: Commands](https://docs.cursor.com/en/agent/chat/commands)
- [Cursor Docs: MCP (Model Context Protocol)](https://docs.cursor.com/en/mcp)

### Quick start

1) Open this project in Cursor
2) Open Chat and type `/` to see available commands
3) Run `/code-review` to get a full review of your branch changes
4) Stage your changes (`git add <files>`), then run `/commit` to generate a Conventional Commit and push

### How commands are discovered

- Cursor automatically loads commands from the `.cursor/commands` directory
- Each command is a Markdown file (`.md`); the file name becomes the slash command (e.g., `commit.md` → `/commit`)
- Optional YAML frontmatter at the top configures the command:
  - `allowed-tools`: which tools the command is permitted to use (e.g., `Bash(git *)`, MCP tools)
  - `description`: shown in the command picker

Example (from this repo):

```1:13:.cursor/commands/code-review.md
---
allowed-tools: Bash(git log:*), Bash(git diff:*), Bash(git show:*), Bash(git branch:*), Bash(git rev-parse:*), Bash(git status:*), Bash(cat:*), Grep, Glob, Read, Write
description: Perform a comprehensive code review of the current branch against the base branch
---
```

### Commands in this repo

- `/code-review`
  - What it does: Analyzes your current Git branch vs the base branch to produce a structured review (change analysis, quality, security, performance, testing, and more)
  - Inputs it gathers: current branch, base branch, uncommitted changes, commits, and file diffs
  - Output: A written review summary and detailed findings; also saved to `/tmp/code_review.md`
  - Requirements: must be in a Git repo with a reachable base branch (e.g., `main`/`master`/`staging`)

- `/commit`
  - What it does: Generates a Conventional Commits message, asks for a brief “why” from you, then commits and pushes
  - Expected flow:
    1. Stage your changes first (`git add <files>`). The command inspects `git status` and `git diff --staged`
    2. It asks you to explain why the change was made; your answer is included in the message
    3. It proposes a commit message (type(scope): subject + rich body)
    4. After you approve, it runs `git commit` and `git push`
  - Notes:
    - It intentionally never adds co-authors
    - If your branch has no upstream set, you may need to set it on first push (Cursor will surface any push errors)
    - If you have an MCP server configured for Jira (see below), the command can enrich context from the ticket

### MCP/Jira (optional)

The `/commit` command lists an MCP tool `mcp__zapier__jira_software_cloud_find_issue_by_key`. If you have a compatible MCP server configured in Cursor, the command can pull Jira ticket details based on your branch name. If not configured, the command still works; it simply skips Jira enrichment. See [Cursor Docs: MCP](https://docs.cursor.com/en/mcp) for setup.

### Sharing on GitHub

To share these commands:
- Keep this folder structure in your repo:

```text
.cursor/
  commands/
    code-review.md
    commit.md
    README.md
```

- Commit and push as usual. Anyone cloning the repo and opening it in Cursor can type `/` to discover and run the commands.

### Troubleshooting

- Don’t see the commands? Verify the directory is exactly `.cursor/commands` and files end with `.md`. Try reloading Cursor
- Git errors on push? Ensure your branch tracks a remote (`git push --set-upstream origin <branch>`) and you’re authenticated
- Base branch detection issues? Make sure your base (`main`/`master`/`staging`) exists locally or is fetchable from origin
