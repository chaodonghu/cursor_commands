---
allowed-tools: Bash(git log:*), Bash(git diff:*), Bash(git show:*), Bash(git branch:*), Bash(git rev-parse:*), Bash(git status:*), Bash(cat:*), Grep, Glob, Read, Write
description: Perform a comprehensive code review of the current branch against the base branch
---

## Analyze Branch Context

First, gather essential information about the current branch:
- Identify the current branch name
- Determine the appropriate base branch (staging, main, or master)
- Check for any uncommitted changes that should be reviewed
- Get the list of all commits in the current branch not in base
- Identify all modified, added, and deleted files

## Perform Comprehensive Code Review

Conduct a thorough review of all changes between the base branch and current branch:

1. **Change Analysis**:
   - Review each modified file with full context
   - Examine the diff for each commit
   - Identify patterns across changes
   - Check for consistency with existing codebase

2. **Code Quality Assessment**:
   - Code style and formatting consistency
   - Variable and function naming conventions
   - Code organization and structure
   - Adherence to DRY (Don't Repeat Yourself) principles
   - Proper abstraction levels

3. **Technical Review**:
   - Logic correctness and edge cases
   - Error handling and validation
   - Performance implications
   - Security considerations (input validation, SQL injection, XSS, etc.)
   - Resource management (memory leaks, connection handling)
   - Concurrency issues if applicable

4. **Best Practices Check**:
   - Design patterns usage
   - SOLID principles adherence
   - Testing coverage implications
   - Documentation completeness
   - API consistency
   - Backwards compatibility

5. **Dependencies and Integration**:
   - New dependencies added
   - Breaking changes to existing interfaces
   - Impact on other parts of the system
   - Database migration requirements

## Generate Review Report

Create a structured code review report with:

1. **Executive Summary**: High-level overview of changes and overall assessment
2. **Statistics**:
   - Files changed, lines added/removed
   - Commits reviewed
   - Critical issues found
3. **Strengths**: What was done well
4. **Issues by Priority**:
   - 🔴 **Critical**: Must fix before merging (bugs, security issues)
   - 🟡 **Important**: Should address (performance, maintainability)
   - 🟢 **Suggestions**: Nice to have improvements
5. **Detailed Findings**: For each issue include:
   - File and line reference
   - Description of the issue
   - Suggested fix or improvement
   - Code example if helpful
6. **Security Review**: Specific security considerations
7. **Performance Review**: Performance implications
8. **Testing Recommendations**: What tests should be added
9. **Documentation Needs**: What documentation should be updated

## User Interaction

After completing the review:
1. Save the review report to `/tmp/code_review.md`
2. Display the complete review report to the user
3. Provide actionable next steps based on findings
4. If critical issues found, highlight them prominently

## Optional Details Integration

If the user provides optional details, use them to:
- Focus on specific areas of concern
- Apply domain-specific review criteria
- Check for compliance with specific requirements
- Validate against architectural decisions
- Review with specific security or performance considerations in mind

## Important Notes
- Be constructive and specific in feedback
- Provide code examples for suggested improvements
- Acknowledge good practices and improvements
- Prioritize issues clearly
- Consider the context and purpose of changes
- Review not just code but also architectural decisions
- Check for potential impacts on other systems
- Ensure review is actionable and helpful