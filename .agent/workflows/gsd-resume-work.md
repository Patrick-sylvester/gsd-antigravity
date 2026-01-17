---
name: gsd-resume-work
description: Resume work from previous session with full context restoration
allowed-tools:
  - Read
  - Bash
  - Write
  - AskUserQuestion
  - SlashCommand
---


> [!IMPORTANT]
> **System Note**: The code snippets in this workflow are written in Bash. The current environment is Windows PowerShell. **You MUST adapt all Bash commands to their PowerShell equivalents before executing them.** Do not try to run Bash directly.


<objective>
Restore complete project context and resume work seamlessly from previous session.

Routes to the resume-project workflow which handles:

- STATE.md loading (or reconstruction if missing)
- Checkpoint detection (.continue-here files)
- Incomplete work detection (PLAN without SUMMARY)
- Status presentation
- Context-aware next action routing
  </objective>

<execution_context>
@C:/Users/jdash/Documents/GSD Google Antigravity/get-shit-done/workflows/resume-project.md
</execution_context>

<process>
**Follow the resume-project workflow** from `@C:/Users/jdash/Documents/GSD Google Antigravity/get-shit-done/workflows/resume-project.md`.

The workflow handles all resumption logic including:

1. Project existence verification
2. STATE.md loading or reconstruction
3. Checkpoint and incomplete work detection
4. Visual status presentation
5. Context-aware option offering (checks CONTEXT.md before suggesting plan vs discuss)
6. Routing to appropriate next command
7. Session continuity updates
   </process>
