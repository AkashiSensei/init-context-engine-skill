---
name: init-context-engine
description: Universal "Independent Thinking" context management system initializer. Creates .context/ files and core engineering protocols in any workspace. Use to bootstrap a new project with structured memory and critical thinking logic.
---

# Skill: Init Context Engine
This skill "bootstraps" a repository with an AI-native engineering environment. It defines how a developer and an AI partner should collaborate using structured memory and critical thinking.

## 1. Directory Structure (Relative & Nested Support)
The system supports multiple context-managed units within a single project (e.g., monorepos or sub-modules).
- **Context Root**: The directory containing the `.context/` folder.
- **Local Memory**:
  - `[Context Root]/.context/RAW_REQUIREMENTS.md`
  - `[Context Root]/.context/SPEC.md`
  - `[Context Root]/.context/ROADMAP.md`
  - `[Context Root]/.context/ACTIVE_TASK.md`
- **Rule of Proximity**: When executing skills, always look for the **nearest** `.context/` directory (climbing up from the current file's path). Fallback to the project root if no local context is found.

## 2. Core Protocols
- **Language**: English for internal docs; Simplified Chinese for user communication.
- **Identity: Independent Thinking Engineer**:
  - Challenge the user on vague or suboptimal instructions.
  - Prioritize "Value over Speed" (Trade-off Analysis).
  - Stop and discuss if motives are unclear.
- **Independence & Context Awareness**: AI must automatically identify the correct `.context/` based on the file being edited or the task's scope.

## 3. Workflow Skills
To maintain this system, implement these core behaviors (as Skills or Slash Commands):
- **sync-spec**: Parse RAW_REQUIREMENTS -> SPEC with logical audit.
- **start-task**: Initialize ACTIVE_TASK based on SPEC/ROADMAP with focus-files.
- **archive-task**: Archive finished tasks to `.context/archive/` and update ROADMAP.

## 4. Initialization & Update (Tool-specific paths)
When running this skill, the agent should perform a "Smart Sync" based on the following rules:

### A. Core Strategy: User-Owned vs. System-Owned
1. **User-Owned Files** (`.context/RAW_REQUIREMENTS.md`, `.context/SPEC.md`, `.context/ROADMAP.md`): 
   - **PRESERVE ONLY**: Never delete or overwrite. 
   - If missing: Create from template. 
   - If exists: Skip or logically merge non-destructive content (e.g., adding missing headings without altering user data).
2. **System-Owned Files** (Protocols in `.cursor/rules/`, Skills in `.cursor/skills/`):
   - **SYNC & UPDATE**: Treat these as "logic engines" that should be kept up to date with the latest global skill templates.
   - If outdated: Update to the latest version from the global `templates/` to ensure the AI's "brain" and "tools" are running the latest logic.
   - If customized: Notify the user before overwriting.

### B. Deployment Path
1. Identify the tool environment:
   - For **Cursor**: Use `.cursor/rules/` and `.cursor/skills/`.
   - For **General AI**: Use project root or `.rules/`.
2. Report the result of the "Smart Sync" (what was created, what was updated, and what was preserved).

[See `templates/` directory for raw markdown files to be copied.]
