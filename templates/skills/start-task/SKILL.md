---
name: start-task
description: Initialize a new development task. Extracts relevant context from SPEC/ROADMAP to ACTIVE_TASK.md.
disable-model-invocation: true
---

# Skill: Start Task
Goal: Create a high-focus "cache" in `ACTIVE_TASK.md` for a specific feature or fix.

## Workflow
1. Identify the **nearest** `.context/` directory based on the user's request.
2. Read `[Context Root]/.context/SPEC.md` and `[Context Root]/.context/ROADMAP.md`.
3. Analyze the current project state and the user's latest request.
3. **Analyze & Challenge (Critical Step)**:
   - Identify if the goal or motivation is unclear. **STOP AND DISCUSS.**
   - **Trade-off Analysis**: Is this the right time to do this? Is the scope appropriate?
   - Identify violations of `SPEC.md` or technical debt. **CHALLENGE AND SUGGEST.**
4. **Draft ACTIVE_TASK.md**:
   - **Focusing Files**: List 3-5 files primary for this task.
   - **Technical Context**: Extract only relevant snippets from SPEC.
   - **Implementation Plan**: Step-by-step TODOs.
   - **Self-exploration**: Note that AI must still explore related files if needed during coding.
5. **Output**: Display the proposed `ACTIVE_TASK.md` and wait for user approval.
