# Commit Helper Acceptance Scenarios

These examples are behavioral checks for the prompt-based skill. Paths are illustrative; each scenario starts with a fresh invocation and no remembered context location.

## 1. Contextless Repository Uses Local Commit Semantics

```text
/work/plain-repo/                 # Git repository root
├── .git/
├── AGENTS.md                     # says messages use "enhance: <summary>"
├── README.md                     # staged product-facing behavior change
└── guide.md                      # unstaged, unrelated change
```

Expected behavior:

- Resolve `/work/plain-repo` with `git rev-parse --show-toplevel` before any context lookup.
- State `contextless / audit-only mode` because the repository has no valid `.context/`.
- Inspect status and the staged diff; leave the unrelated unstaged `guide.md` untouched.
- Skip `ACTIVE_TASK.md`, `ROADMAP.md`, and Auto-Sync entirely, and do not create `.context/`.
- Use the repository's `enhance` taxonomy. Do not choose `docs` merely because the staged file is Markdown.
- Commit only the already staged `README.md` after the final staged-diff audit.

## 2. Repository Boundary Excludes Parent and Sibling Context

```text
/work/
├── .context/                     # unrelated parent context
├── repo-a/
│   ├── .git/
│   └── .context/                 # repo-a context
└── repo-b/                       # current working repository
    ├── .git/
    └── src/change.ts             # staged
```

Expected behavior when invoked in `/work/repo-b`:

- Resolve `REPO_ROOT=/work/repo-b` first and stop every context walk at that path.
- Do not inspect or reuse `/work/.context` or `/work/repo-a/.context`.
- Enter contextless / audit-only mode and perform the normal status, staged-diff, scope, and commit-safety checks.
- Create or modify no context files anywhere.

The same result is required when `/work/repo-b/.context` is a symlink resolving to `/work/repo-a/.context`: reject the candidate because its canonical path is outside `REPO_ROOT`.

## 3. Intentional In-Repository Context Preserves Auto-Sync

```text
/work/contextful-repo/            # Git repository root
├── .git/
├── .context/
│   ├── README.md                 # project-context governance
│   ├── ACTIVE_TASK.md
│   └── ROADMAP.md
└── src/change.ts                 # staged task change
```

Expected behavior:

- Resolve the Git root, accept the canonical in-repository `.context/`, and state `contextful mode`.
- Read `.context/README.md` before the other context files.
- Apply the existing Auto-Sync rules to completed task items and finalized milestones.
- Stage only context files actually changed by Auto-Sync, then re-audit the complete staged diff before committing.

## 4. Domain Taxonomy Overrides File Heuristics

```text
/work/domain-repo/
├── .git/
├── CONTRIBUTING.md               # allowed types: model, pipeline, eval
└── prompts/ranker.md              # staged ranking-behavior change
```

Expected behavior:

- Treat `CONTRIBUTING.md` as authoritative for message semantics.
- Select the domain type that matches the behavioral intent, such as `model`, only when supported by the declared convention and diff.
- Do not emit `docs` from the `.md` extension and do not invent `feat` as a generic fallback.
- Keep staged-scope and commit-safety decisions independent from this taxonomy decision.

## 5. Exact Committed Message Is Shown Immediately

After a successful commit, assume Git stores this message, whether unchanged from the draft or rewritten by a commit hook:

```text
enhance: bound context discovery to the repository

Keep contextless commits independent from project memory.
```

Expected behavior:

- Run `git log -1 --format='%H%n%B'` and treat that output, not the earlier draft, as authoritative.
- Immediately show the resulting hash and complete subject/body to the user in a clearly labeled code block.
- Do not paraphrase or truncate the message, and do not wait for a later PR summary to reveal it.
- If the user spots a problem, leave the commit unchanged until the user explicitly requests an amend or replacement.

## Review Checklist

- Git root is resolved before any context lookup.
- Context discovery and canonical paths never cross the Git root.
- Contextless mode performs no context discovery beyond determining that no valid in-repository candidate exists, no context mutation, and no context synthesis.
- Contextful mode retains governed Auto-Sync.
- Both modes retain status, staged-diff, exact-scope, and final-index audits.
- Repository-local conventions outrank recent history; extensions never determine commit type by themselves.
- Every successful commit is followed immediately by its exact Git-stored hash and full message for user review.
