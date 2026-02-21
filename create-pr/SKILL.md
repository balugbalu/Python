---
name: create-pr
description: Creates a GitHub Pull Request from the current branch to a specified target branch (e.g. master, release branch). Generates a structured PR with title, Jira reference, description, root cause/fix analysis, implementation details, and test results. Use when the user asks to create a PR or pull request.
allowed-tools: Bash(git:*) Bash(gh:*)
---

# Create Pull Request

## When to use
Use this skill when the user asks to create a PR or pull request for the current branch.

## Prerequisites
- GitHub CLI (`gh`) must be installed and authenticated
- Must be inside a git repository

## Steps

1. **Determine target branch**: Ask the user or parse from their request (default: `master`).
2. **Get current branch name**: Run `git branch --show-current`.
3. **Analyze changes**: Run `git log --oneline $(git merge-base HEAD origin/<target>)..HEAD` and `git diff origin/<target>...HEAD --stat` to understand what changed.
4. **Extract Jira ID**: Look for a Jira ticket pattern (e.g. `PROJ-1234`) in branch name or commit messages.
5. **Generate PR content** using the template in [references/pr-template.md](references/pr-template.md).
6. **Determine PR type**: Based on commits and diff, classify as Bug Fix or New Feature.
   - If **Bug Fix**: Fill in Root Cause and Fix sections.
   - If **New Feature**: Fill in Implementation Details section.
7. **Check for common/shared functions**: If new utility or common functions were added, include usage examples.
8. **List test cases**: Summarize new/modified tests and their results.
9. **Create the PR**:
```bash
gh pr create --base <target-branch> --title "<title>" --body "<body>"