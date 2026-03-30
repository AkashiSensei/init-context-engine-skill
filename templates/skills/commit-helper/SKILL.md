---
name: commit-helper
description: Generate professional Git commit commands and PR descriptions by analyzing staged changes and project context.
disable-model-invocation: true
---

# Skill: Commit Helper
Goal: Deliver high-density commit messages and PR descriptions while auto-syncing project context.

## Workflow
1. **Context Extraction**:
   - Run `git diff --staged` and `git status`.
   - Run `git --no-pager log -n 5 --oneline` to identify project commit style.
   - For PR descriptions: Identify the base branch (e.g., `main`) and run `git --no-pager log [base]..HEAD --oneline` and `git diff [base]...HEAD` to capture the full scope of changes across multiple commits.
   - Locate nearest `.context/` and identify task intent from `ACTIVE_TASK.md`.
2. **Auto-Sync**:
   - Update `ACTIVE_TASK.md` (auto-check completed items).
   - Log significant milestones/decisions in `ROADMAP.md`.
3. **Drafting**:
   - **Commit Command**: Generate `git commit -m "[message]"` following project rules (or `type(scope): desc`).
   - **PR Description** (Triggered by `archive-task` or explicit request):
     - Wrap in a code block.
     - Language: Match current conversation.
     - Scope: Analyze all changes since diverging from the base branch (not just the current commit).
     - Content: Concise intent, bulleted changes, test summary, and issue refs (from `ACTIVE_TASK.md` and commit history).
4. **Output**: Present command and PR text for review. **DO NOT** execute or run `git add`.

## Interaction Style
- Technical, concise, zero fluff.
- Query user first if changes appear ambiguous.
