---
name: gsd-prepare-testing
description: Analyze codebase and generate testing workflows for each feature
argument-hint: "[optional: specific feature or component to focus on]"
---


> [!IMPORTANT]
> **System Note**: The code snippets in this workflow are written in Bash. The current environment is Windows PowerShell. **You MUST adapt all Bash commands to their PowerShell equivalents before executing them.** Do not try to run Bash directly.


<objective>
Analyze the project and generate comprehensive testing workflows in the `.testing/` folder.

This is the PREPARATION phase of testing. It:
1. Scans the codebase to discover testable features
2. Reviews `.planning/` documents for context
3. Generates structured test workflow files
4. Creates the `.backlog/` folder structure for bug tracking

Output: `.testing/` folder with test workflow files, `.backlog/` ready for bugs
</objective>

<execution_context>
@C:/Users/jdash/Documents/GSD Google Antigravity/get-shit-done/skills/testing-agent/SKILL.md
@C:/Users/jdash/Documents/GSD Google Antigravity/get-shit-done/skills/testing-agent/scripts/analyze-project.md
@C:/Users/jdash/Documents/GSD Google Antigravity/get-shit-done/skills/testing-agent/templates/test-workflow.md
</execution_context>

<context>
Optional scope: $ARGUMENTS
- If provided: Focus testing prep on specific feature/component
- If not provided: Analyze entire project

**Load project context if available:**
@.planning/PROJECT.md (optional - may not exist)
@.planning/ROADMAP.md (optional - may not exist)
@.planning/STATE.md (optional - may not exist)
</context>

<process>
<step name="setup">
**Create testing infrastructure:**

1. Create `.testing/` directory if it doesn't exist
2. Create `.backlog/simple/` and `.backlog/complex/` directories
3. Create `.backlog/README.md` with backlog overview

```powershell
New-Item -ItemType Directory -Path ".testing", ".backlog/simple", ".backlog/complex" -Force
```
</step>

<step name="discover">
**Discover testable features:**

Follow the patterns in `analyze-project.md`:

1. **Page Discovery**: Find all routes/pages in the project
   - Look for Next.js app router: `src/app/**/page.tsx`
   - Look for pages directory: `pages/**/*.tsx`
   - Look for other frameworks as appropriate

2. **Component Discovery**: Find interactive elements
   - Forms with onSubmit handlers
   - Buttons with onClick handlers
   - Modals, drawers, dialogs
   - Navigation elements

3. **API Discovery**: Find backend endpoints
   - API route files
   - Fetch/axios calls in frontend

4. **Flow Discovery**: Read `.planning/` for documented user journeys

Present a summary of discovered features to understand scope.
</step>

<step name="categorize">
**Categorize features into test groups:**

Group by:
- **auth-testing.md** - Login, logout, signup, password flows
- **[feature]-testing.md** - Main feature areas (dashboard, settings, etc.)
- **api-testing.md** - Backend endpoint validation (if applicable)
- **edge-cases-testing.md** - Error states, empty states, boundaries

Each group becomes one test workflow file.
</step>

<step name="generate">
**Generate test workflow files:**

For each category:

1. Use the template from `test-workflow.md`
2. Create specific test cases based on discovered features
3. Include:
   - Clear pre-conditions
   - Step-by-step test instructions
   - Expected results
   - Pass/fail criteria
4. Order tests logically (setup → core → edge cases → cleanup)

Save to `.testing/[category]-testing.md`
</step>

<step name="readme">
**Create testing README:**

Create `.testing/README.md` with:

```markdown
# Testing Workflows

Generated: [date]
Project: [project name from PROJECT.md or directory name]

## Available Test Workflows

| File | Coverage | Test Cases |
|------|----------|------------|
| [filename] | [what it covers] | [count] |

## How to Run Tests

1. Start the development server
2. Run: `/gsd:start-testing [filename]`
3. Review bugs in `.backlog/`

## Bug Tracking

- Simple bugs: `.backlog/simple/`
- Complex bugs: `.backlog/complex/`
- Run `/gsd:start-debugging simple` or `complex` to address bugs
```
</step>

<step name="backlog-readme">
**Create backlog README:**

Create `.backlog/README.md`:

```markdown
# Bug Backlog

This directory contains bugs discovered during automated testing.

## Structure

- `simple/` - Quick fixes, typically < 30 minutes
  - Visual/CSS issues
  - Missing text or broken links
  - Minor component rendering problems
  
- `complex/` - Deeper investigation required
  - Logic errors
  - State management issues
  - Multi-step workflow failures
  - API integration problems

## Working the Backlog

Run `/gsd:start-debugging simple` or `/gsd:start-debugging complex`

## Bug Report Format

Each bug file follows the template in:
`get-shit-done/skills/testing-agent/templates/bug-report.md`
```
</step>

<step name="summary">
**Present summary:**

Show the user:
1. Number of pages/components discovered
2. Test workflow files created
3. Total test cases generated
4. Recommended testing order

Offer next action: `/gsd:start-testing [first-workflow.md]`
</step>
</process>

<anti_patterns>
- Don't generate tests without analyzing the actual codebase
- Don't create empty or placeholder test cases
- Don't skip the `.backlog/` setup
- Don't make assumptions about features - verify they exist
</anti_patterns>

<success_criteria>
- [ ] `.testing/` directory created with README
- [ ] `.backlog/simple/` and `.backlog/complex/` directories created
- [ ] `.backlog/README.md` created
- [ ] At least one test workflow file generated per major feature area
- [ ] Each test workflow has proper structure with test cases
- [ ] User knows how to proceed with testing
</success_criteria>
