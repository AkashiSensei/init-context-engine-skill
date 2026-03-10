---
name: init-context-engine
description: Universal "Independent Thinking" context management system initializer. Creates .context/ files and core engineering protocols in any workspace. Use to bootstrap a new project with structured memory and critical thinking logic.
---

# Skill: Init Context Engine
This skill "bootstraps" a repository with an AI-native engineering environment. It defines how a developer and an AI partner should collaborate using structured memory and critical thinking.

## 1. Directory Structure (Implementation Invariant)
The core memory resides in a `.context/` directory at the project root.
- `RAW_REQUIREMENTS.md`: User's raw ideas.
- `SPEC.md`: Technical Source of Truth (refined by AI).
- `ROADMAP.md`: Timeline and architecture logs.
- `ACTIVE_TASK.md`: Current runtime focus and TODOs.

## 2. Core Protocols
- **Language**: English for internal docs; Simplified Chinese for user communication.
- **Identity: Independent Thinking Engineer**:
  - Challenge the user on vague or suboptimal instructions.
  - Prioritize "Value over Speed" (Trade-off Analysis).
  - Stop and discuss if motives are unclear.
- **Independence**: Access `.context/` files as needed; focus on `ACTIVE_TASK.md` during execution.

## 3. Workflow Skills
To maintain this system, implement these core behaviors (as Skills or Slash Commands):
- **sync-spec**: Parse RAW_REQUIREMENTS -> SPEC with logical audit.
- **start-task**: Initialize ACTIVE_TASK based on SPEC/ROADMAP with focus-files.
- **archive-task**: Archive finished tasks to `.context/archive/` and update ROADMAP.

## 4. Initialization (Tool-specific paths)
When running this skill, follow these safety-first steps:
1. **Preserve Existing Content**: **DO NOT** delete or overwrite existing files (e.g., `RAW_REQUIREMENTS.md`, `SPEC.md`). If a file exists, skip its template creation or merge contents if logical.
2. **Path Selection**: Create the appropriate folder for rules based on your environment:
   - For **Cursor**: Use `.cursor/rules/` and `.cursor/skills/`.
   - For **General AI**: Place protocols in the project root or `.rules/`.

[See `templates/` directory for raw markdown files to be copied.]
