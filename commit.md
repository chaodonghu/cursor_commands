---
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git push:*), Bash(git branch:*), mcp__zapier__jira_software_cloud_find_issue_by_key
description: Generate a Conventional Commits message based on changes and commit + push
---

IMPORTANT: NEVER EVER ADD CO-AUTHOR TO THE GIT COMMIT MESSAGE

## Context
- Current git status: !`git status`
- Current git diff: !`git diff --staged`
- Recent commits for context: !`git log --oneline -5`
- Current branch: !`git branch --show-current`

## Workflow

1. **Extract Jira Ticket Context** (if applicable):
   - Parse the current branch name to find Jira ticket IDs (patterns: PROJ-123-feature-name, feature/PROJ-123-description, or PROJ-123)
   - If a Jira ticket ID is found, use MCP to fetch ticket details (title, description, acceptance criteria, comments)
   - Use this context to understand the broader purpose of the changes

2. **Human-in-the-Loop - Ask for Context**:
   Before generating the commit message, ALWAYS ask the user:

   "Can you briefly explain WHY you made this change and what problem it solves?"

   Wait for their response and incorporate their explanation into the commit message.

3. **Analyze Technical Changes**:
   Review the staged changes to understand WHAT changed technically (files modified, functions added/updated, etc.)

4. **Create Enhanced Commit Message**:
   Generate a commit message that tells a complete story for future "Zapier archeology":

   Format:
   ```
   type(scope): concise subject line describing what changed

   Why this change was needed:
   [Incorporate user's explanation and Jira ticket context]

   What changed:
   [Technical summary of the modifications]

   Problem solved:
   [Explain the business/technical problem this addresses]

   [If Jira ticket found] Refs: PROJ-123
   ```

5. **Conventional Commits Types**:
   - feat: new features
   - fix: bug fixes
   - docs: documentation changes
   - style: formatting, missing semicolons, etc.
   - refactor: code restructuring without changing functionality
   - test: adding or updating tests
   - chore: maintenance tasks, dependencies, build process
   - perf: performance improvements
   - ci: continuous integration changes

6. **Execute Commit and Push**:
   **Do not use git add -A or git add .. Commit only the files you've edited and understand the context around.**

   After receiving user approval of the commit message:
   1. `git commit -m "your-generated-message"` (use heredoc for multi-line)
   2. `git push`

## Storytelling Emphasis

Create commit messages that future developers (or future you) will appreciate when doing "Zapier archeology" months later. The message should answer:
- **What** changed? (technical summary)
- **Why** was this needed? (business context, user explanation)
- **What problem** does it solve? (from Jira ticket and user input)
- **How** does it relate to the larger feature/project? (Jira context)

NEED TO REMEMBER: Do not EVER include any co-author information in the commit and don't mention claude code.
