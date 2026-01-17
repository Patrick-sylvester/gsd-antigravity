---
name: gsd-start-debugging
description: Work through bugs in a backlog folder (simple or complex)
argument-hint: "<simple|complex> - which backlog to work"
---


> [!IMPORTANT]
> **System Note**: The code snippets in this workflow are written in Bash. The current environment is Windows PowerShell. **You MUST adapt all Bash commands to their PowerShell equivalents before executing them.** Do not try to run Bash directly.


<objective>
Systematically work through bugs in a backlog folder, fixing and verifying each one.

This is the RESOLUTION phase of testing. It:
1. Lists all bugs in the specified backlog folder
2. For each bug: analyze, fix, verify, and archive
3. Escalates bugs if scope changes during investigation
4. Commits each fix atomically

Output: Fixed bugs, atomic commits, updated backlog
</objective>

<execution_context>
@C:/Users/jdash/Documents/GSD Google Antigravity/get-shit-done/skills/testing-agent/SKILL.md
</execution_context>

<context>
Required: $ARGUMENTS = "simple" or "complex"

**Backlog locations:**
- Simple: `.backlog/simple/`
- Complex: `.backlog/complex/`
</context>

<process>
<step name="validate">
**Validate inputs:**

1. Check that $ARGUMENTS is "simple" or "complex"
   - If not provided or invalid: Ask which backlog to work

2. Check that backlog folder exists and has bugs:

```powershell
$backlogPath = ".backlog/$ARGUMENTS"
$bugs = Get-ChildItem -Path $backlogPath -Filter "*.md" -ErrorAction SilentlyContinue
if ($bugs.Count -eq 0) {
    Write-Host "No bugs in $backlogPath"
}
```

3. If no bugs: Report completion and exit
</step>

<step name="list">
**List and prioritize bugs:**

1. Read all bug files in the backlog folder
2. Sort by:
   - Severity (Blocker > Major > Minor > Cosmetic)
   - Then by bug number (oldest first)
3. Present list to user:

```markdown
# Bug Backlog: [simple/complex]

Found [N] bugs to address:

| ID | Severity | Component | Title |
|----|----------|-----------|-------|
| BUG-001 | Major | Auth | Login button not responding |
| BUG-003 | Minor | Settings | Wrong icon color |
| ...

Starting with highest priority: BUG-[NNN]
```
</step>

<step name="work-bug">
**For each bug (in priority order):**

### 1. Load Bug Report
Read the full bug report file to understand:
- Steps to reproduce
- Expected vs actual behavior
- Suspected files
- Any technical notes

### 2. Analyze the Code
Find the relevant code:
- Check the "Files Likely Involved" section
- Search codebase for related components
- Understand the current implementation

### 3. Implement Fix
Make the necessary code changes:
- Focus on minimal, targeted changes
- Follow existing code patterns
- Don't introduce new dependencies unless essential

### 4. Verify Fix
Use browser_subagent to verify:
- Follow the "Steps to Reproduce" from bug report
- Confirm the issue is resolved
- Check for regressions in related functionality

### 5. Handle Result

**If fix successful:**
- Update bug report status to "Fixed"
- Add resolution notes
- Move bug file to `.backlog/archived/` (create if needed)
- Git commit with message: `fix(BUG-NNN): [brief description]`

**If fix unsuccessful:**
- Note what was tried
- If scope increased: Move to complex backlog
- If needs more info: Mark as "Blocked" and continue

**If was simple, now complex:**
```powershell
# Move bug to complex backlog
Move-Item ".backlog/simple/BUG-NNN-slug.md" ".backlog/complex/"
Write-Host "Bug escalated to complex backlog"
```
Add a note explaining why scope changed.
</step>

<step name="escalate">
**Handle scope escalation (simple → complex):**

When a "simple" bug turns out to be complex:

1. Add a section to the bug report:
```markdown
## Escalation Notes

**Escalated**: [date]
**Reason**: [why this is actually complex]
- [specific technical reason]
- [what investigation revealed]

**Original Assessment**: Simple (visual issue)
**Revised Assessment**: Complex (requires [explanation])
```

2. Move the file:
```powershell
Move-Item ".backlog/simple/BUG-NNN-slug.md" ".backlog/complex/"
```

3. Continue to next bug in simple backlog
</step>

<step name="commit">
**Atomic commits for each fix:**

After each successful fix:

```powershell
git add -A
git commit -m "fix(BUG-NNN): [brief description of fix]"
```

Commit message format:
- `fix(BUG-001): correct login button click handler`
- `fix(BUG-003): update settings icon color to match design`

This keeps each fix independently revertible.
</step>

<step name="archive">
**Archive completed bugs:**

Create `.backlog/archived/` if it doesn't exist.

Move fixed bugs:
```powershell
New-Item -ItemType Directory -Path ".backlog/archived" -Force
Move-Item ".backlog/simple/BUG-NNN-*.md" ".backlog/archived/"
```

Keep the bug report for history - don't delete.
</step>

<step name="summary">
**Present session summary:**

```markdown
# Debugging Session Complete

**Backlog**: [simple/complex]
**Session Duration**: [approximate time]

## Results

| Outcome | Count |
|---------|-------|
| ✅ Fixed | [N] |
| ↗️ Escalated | [N] |
| ⏸️ Blocked | [N] |
| ⏭️ Skipped | [N] |

## Bugs Fixed
- BUG-001: [title] - commit abc123
- BUG-003: [title] - commit def456

## Bugs Escalated (moved to complex)
- BUG-007: [title] - [reason]

## Bugs Remaining in Backlog
- [N] bugs still in [simple/complex] backlog

## Next Steps
[Recommendations based on results]
```
</step>

<step name="offer">
**Offer next actions:**

Based on results:
- If more bugs in current backlog: Offer to continue
- If current empty, other has bugs: Suggest other backlog
- If bugs escalated to complex: Note they need attention
- If all done: Celebrate completion!

Options:
- "Continue debugging [simple/complex]"
- "Switch to [other] backlog"
- "Run more tests to find new bugs"
- "Done for now"
</step>
</process>

<anti_patterns>
- Don't fix multiple bugs in one commit - atomic commits only
- Don't delete bug reports - archive them
- Don't skip verification - actually test the fix
- Don't force a fix that doesn't work - escalate or mark blocked
- Don't stay stuck on one bug forever - timebox and move on
</anti_patterns>

<success_criteria>
- [ ] All bugs in backlog were attempted
- [ ] Each fix was verified via browser
- [ ] Successful fixes have atomic commits
- [ ] Fixed bugs archived with status updated
- [ ] Scope changes properly escalated
- [ ] Summary shows accurate results
- [ ] User knows remaining work
</success_criteria>
