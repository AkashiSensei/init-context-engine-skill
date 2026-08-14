---
name: commit-helper
description: Commit task-relevant changes safely using repository-local conventions, with optional in-repository project-context Auto-Sync, and generate PR descriptions.
disable-model-invocation: true
---

# Skill: Commit Helper

Goal: Commit a narrow, reviewed scope with a repository-native message. Project-context Auto-Sync is optional and must never cross the current Git repository boundary.

## Non-Negotiable Repository Boundary

1. Before looking for `.context/`, `ACTIVE_TASK.md`, or `ROADMAP.md`, run `git rev-parse --show-toplevel` and canonicalize the result as `REPO_ROOT`.
2. If the command fails, stop: this skill requires a Git repository. Do not search parent directories for context.
3. Treat `REPO_ROOT` as a hard read/write boundary for project-context discovery:
   - Apply the proximity rule only by walking from each relevant in-repository path toward `REPO_ROOT`, stopping after `REPO_ROOT`.
   - Never run a context search from a parent of `REPO_ROOT`, reuse a remembered context path, or inspect a sibling repository.
   - Canonicalize every `.context/` candidate. Reject a symlink or other candidate whose resolved path is outside `REPO_ROOT`.
   - A usable context directory must be inside `REPO_ROOT` and contain its project-context `README.md`. If relevant paths resolve to different context directories, stop and ask which project scope should be committed.
4. Select and state one mode explicitly:
   - **Contextful mode**: a valid, unambiguous in-repository `.context/` exists. Read its `README.md` first, then use only the context files it declares.
   - **Contextless / audit-only mode**: no valid in-repository `.context/` exists. Skip all `ACTIVE_TASK.md` and `ROADMAP.md` discovery, reads, writes, staging, and Auto-Sync. Do not create `.context/` or synthesize any context file.

Here, **audit-only** describes context handling. It does not prevent an explicitly requested Git commit after the repository-state and scope audits pass.

## Commit-Safety Mechanics

These checks apply in both modes and are independent of commit-message taxonomy.

1. Run `git status --short` and `git diff --staged`; record the exact paths that were already staged before this skill changes anything.
2. Determine a narrow task scope:
   - If changes are already staged, verify that every staged hunk belongs to the requested task.
   - If nothing is staged, inspect `git diff --stat`, `git diff`, and the exact untracked paths relevant to the request.
   - In contextful mode, `ACTIVE_TASK.md` may clarify intent. In contextless mode, use the user request, repository-local instructions, and the diff only.
3. Preserve pre-existing staged work. If it is unrelated, ambiguous, incomplete, or mixed with the requested task, stop and ask before changing the index or committing.
4. If staging is required, stage exact reviewed paths only. Never use broad commands such as `git add .` or `git add -A`.
5. After any allowed Auto-Sync, inspect `git status --short` and `git diff --staged` again. Commit only when the final staged diff is clear, complete, and limited to the intended scope.

## Commit-Message Semantics

Choose message semantics separately from the safety audit.

1. Read authoritative conventions only from inside `REPO_ROOT`, in this order:
   - the user's explicit instructions;
   - repository-local `AGENTS.md` files applicable to the changed paths;
   - `CONTRIBUTING.md` or another repository-declared commit convention, such as commitlint/release configuration;
   - recent repository history from `git --no-pager log -n 5 --oneline` when no stronger rule defines the style.
2. Follow a repository-defined domain taxonomy exactly. Use `type(scope): description` only when the repository's rules or stable history actually use that form.
3. Infer the semantic category from the intent and observable behavior of the change under that taxonomy. Do **not** infer `docs`, `feat`, or another type merely from file extensions or directory names. A Markdown change can implement a product capability, and a source-file change can be documentation or maintenance.
4. If the repository declares no taxonomy and history has no stable one, use a concise imperative summary without inventing a Conventional Commit type.

## Contextful Auto-Sync

Run this section only in contextful mode.

1. Use task intent from the selected context directory's existing `ACTIVE_TASK.md`, if present.
2. Update existing context files according to that directory's `README.md`:
   - check completed items in `ACTIVE_TASK.md`;
   - log significant completed milestones or finalized decisions in `ROADMAP.md`.
3. Do not invent a missing context file unless its in-repository governance explicitly authorizes creation. `ACTIVE_TASK.md` remains owned by the `start-task` workflow.
4. Stage only the context files changed by this skill, from the selected in-repository context directory, so the task record is committed with the implementation.

This preserves Auto-Sync for repositories that intentionally use the project-context system while making it impossible for a contextless repository to borrow another project's state.

## Commit and PR Execution

1. Draft the commit message using the semantic rules above and the final staged diff.
2. Commit the union of the task-relevant paths staged before this skill ran, any exact task paths staged after review, and context files staged by contextful Auto-Sync.
3. After the final staged diff passes the audit, run `git commit -m "[message]"` directly.
4. Immediately after a successful commit, read back the actual commit with `git log -1 --format='%H%n%B'`. Treat Git's stored hash and full message as authoritative because hooks may have modified the draft.
5. For a PR description, when explicitly requested or triggered by `archive-task`:
   - Identify the base branch and inspect `git --no-pager log [base]..HEAD --oneline` plus `git diff [base]...HEAD` for the complete branch scope.
   - In contextful mode, existing task context may supply intent and issue references. In contextless mode, never look outside the repository for them; use the request, repository history, and diff.
   - Return a reviewable code block in the conversation language with concise intent, bulleted changes, tests, and verified issue references.

## Output

Immediately after committing, show the selected mode, commit hash, and the exact full commit message read back from Git. Present the message verbatim in a clearly labeled code block, including any body; do not paraphrase, truncate, or defer it until a later summary. This is a user review checkpoint: if the user identifies a problem, do not amend or replace the commit unless explicitly requested. Mention context files only when contextful Auto-Sync actually changed them. For PR descriptions, present the generated text for review.

See [SCENARIOS.md](SCENARIOS.md) for contextful, contextless, repository-boundary, and message-taxonomy acceptance examples.

## Interaction Style

- Technical, concise, zero fluff.
- Ask before staging or committing when scope remains ambiguous.
