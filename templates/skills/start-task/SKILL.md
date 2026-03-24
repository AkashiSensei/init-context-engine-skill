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
   - **Trade-off Analysis**: Is this the right time? Is the scope appropriate?
   - Identify violations of current architecture or technical debt. **CHALLENGE AND SUGGEST.**
4. **Planning Phase (Documents Only)**:
   - **Gap Analysis**: Proactively update `SPEC.md` and `RAW_REQUIREMENTS.md` only if:
     - The task is entirely missing from these documents (ensures source of truth).
     - The task introduces new global requirements or architectural constraints.
     - *Think like an Engineer*: Evaluate if updating SPEC is truly necessary for the project's long-term health. Avoid cluttering SPEC with implementation details.
   - **Isolation**: During the planning stage, ALL modifications must be restricted to `.context` files (ACTIVE_TASK.md, SPEC.md, RAW_REQUIREMENTS.md). **DO NOT** modify any source code until explicitly authorized.
5. **Draft ACTIVE_TASK.md**:
   - **Goal**: Concise one-liner.
   - **Issue Reference**: Capture any Issue links or IDs provided by the user.
   - **Focusing Files**: List 3-5 primary files.
   - **Implementation Details**: Capture specific functional requirements and implementation ideas here.
   - **Test Plan**: Define the testing strategy (Unit/UI/Manual) for this specific task.
   - **Technical Context**: Extract ONLY essential snippets/constraints from SPEC.
   - **TODOs**: High-density implementation steps.
6. **Output & Approval**: Display the proposed plan and wait for the user to explicitly say "Start Implementation" or approve the plan before touching any code.
