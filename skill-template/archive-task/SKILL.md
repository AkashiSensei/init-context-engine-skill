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
3. **Archive Process**:
   - **Mark Completion**: Update the `ACTIVE_TASK.md` content in memory to reflect its finished state (e.g., add `Status: COMPLETED on YYYY-MM-DD` and ensure all checkpoints are ticked).
   - **File Migration**: Save the modified content to `[Context Root]/.context/archive/YYYYMMDD_TaskName.md` and remove/reset the current active file.
4. **Update ROADMAP.md**: Add a single-line entry: `Date | [TaskName](archive/YYYYMMDD_TaskName.md) | Key Impact`.
5. **Reset**: Re-create a skeletal `[Context Root]/.context/ACTIVE_TASK.md` containing only the header `# ACTIVE_TASK`.
6. **Trigger commit-helper**: Call `commit-helper` to finalize the work in Git and generate a PR description.
7. Output: "Task archived. Roadmap updated. Committing changes..."
