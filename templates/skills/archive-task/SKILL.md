---
name: archive-task
description: Archive the active task and update the roadmap. Only use when explicitly invoked.
disable-model-invocation: true
---

# Skill: Archive Task
Cleanup the workspace after a task is completed.

## Workflow
1. Identify the **nearest** `.context/` directory.
2. Read `[Context Root]/.context/ACTIVE_TASK.md` and `[Context Root]/.context/ROADMAP.md`.
3. Move content of `ACTIVE_TASK.md` to `[Context Root]/.context/archive/YYYYMMDD_TaskName.md`.
4. Update `ROADMAP.md`: Add a single-line entry: `Date | TaskName | Key Impact`.
5. Reset `ACTIVE_TASK.md`: Keep only the minimalist template for the next task.
6. **Trigger commit-helper**: Call `commit-helper` to finalize the work in Git and generate a PR description.
7. Output: "Task archived. Roadmap updated. Committing changes..."
