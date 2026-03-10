---
name: sync-spec
description: Sync RAW_REQUIREMENTS.md to SPEC.md. Only use when explicitly invoked.
disable-model-invocation: true
---

# Skill: Sync Spec
Goal: Synchronize user's raw thoughts into the structured technical specification.

## Workflow
1. Read `.context/RAW_REQUIREMENTS.md` and `.context/SPEC.md`.
2. Extract new/changed items from RAW.
3. **Analyze & Question (Critical Step)**:
   - Identify if the user's goal or motivation is unclear. **STOP AND DISCUSS.**
   - **Trade-off Analysis**: Is this the right time to do this? Is the scope appropriate?
   - Identify violations of current architecture or technical debt. **CHALLENGE AND SUGGEST.**
4. **Update SPEC.md**:
   - Refine the technical specification based on the new requirements.
   - **Dynamic Structure**: AI must decide the structure of `SPEC.md` based on the project's current stage.
   - **Concise**: Keep `SPEC.md` as short as possible while remaining precise.
5. **Output**: Display the proposed `SPEC.md` and wait for user approval.
