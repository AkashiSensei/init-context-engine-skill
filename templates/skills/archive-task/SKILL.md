---
name: archive-task
description: Archive the active task and update the roadmap. Only use when explicitly invoked.
disable-model-invocation: true
---

# Skill: Archive Task
Cleanup the workspace after a task is completed.

## Workflow
1. Read `.context/ACTIVE_TASK.md` and `.context/ROADMAP.md`.
2. Move content of `ACTIVE_TASK.md` to `.context/archive/YYYYMMDD_TaskName.md`.
3. Update `ROADMAP.md`: Add a single-line entry: `Date | TaskName | Key Impact`.
4. Reset `ACTIVE_TASK.md`: Keep only the minimalist template for the next task.
5. Output: "Task archived. Roadmap updated."
