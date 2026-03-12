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
3. **Analyze & Challenge (Critical Step)**:
   - Identify if the goal or motivation is unclear. **STOP AND DISCUSS.**
   - **Gap Analysis**: If the task is NOT in `SPEC.md` or `RAW_REQUIREMENTS.md`, **proactively update** both files to ensure architectural alignment before proceeding.
   - **Trade-off Analysis**: Is this the right time? Is the scope appropriate?
   - Identify violations of `SPEC.md` or technical debt. **CHALLENGE AND SUGGEST.**
4. **Draft ACTIVE_TASK.md**:
   - **Goal**: Concise one-liner.
   - **Focusing Files**: List 3-5 primary files.
   - **Technical Context**: Extract ONLY essential snippets/constraints.
   - **TODOs**: High-density implementation steps.
5. **Output**: Display the proposed `ACTIVE_TASK.md` and wait for approval.
