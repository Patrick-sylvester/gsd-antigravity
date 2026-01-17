---
name: gsd-compact
description: Compact context to prevent context window rot and maintain agent performance
---


> [!IMPORTANT]
> **System Note**: This workflow helps combat context window degradation in long sessions by summarizing accumulated context into STATE.md.


<objective>
Compact the current session context to prevent context window rot.

This workflow:
1. Reviews the current STATE.md and recent work
2. Identifies critical context that must persist
3. Creates a compressed summary of recent decisions
4. Updates STATE.md with session checkpoint
5. Provides a clean "continue from here" point

Use this:
- Before complex multi-phase work
- After extended debugging sessions
- When you notice context degradation
- Proactively every 20+ turns in a session
</objective>

<context>
**Load project state:**
@.planning/STATE.md
@.planning/ROADMAP.md (optional)
</context>

<process>
<step name="assess">
**Assess current context state:**

Review:
1. How much context has accumulated in this session
2. What major decisions have been made
3. What files/components are being actively worked
4. What issues remain unresolved

Identify what MUST persist vs what can be summarized.

**Must Persist:**
- Active bugs being investigated
- Uncommitted code changes
- Decisions that affect next steps
- Configuration choices made
- Blockers or waiting items

**Can Summarize:**
- Completed tasks
- Investigated but rejected approaches  
- Routine file edits
- Successful verifications
</step>

<step name="summarize">
**Create session summary:**

Generate a compact summary of the session:

```markdown
## Session Summary: [Date/Time]

### Completed
- [Major accomplishment 1]
- [Major accomplishment 2]

### Active Work
- Currently: [what's in progress]
- Files: [key files being modified]

### Decisions Made
- [Important decision 1]: [rationale]
- [Important decision 2]: [rationale]

### Known Issues
- [Issue 1]: [status/next step]

### Next Steps
1. [Immediate next action]
2. [Following action]
```
</step>

<step name="update-state">
**Update STATE.md:**

Add the session summary to STATE.md under a "Session Continuity" section:

If section exists, replace it. If not, add it.

```markdown
## Session Continuity

**Last Checkpoint:** [timestamp]

[Session summary content]

### Context Restoration Notes

When resuming:
1. [Key file to check first]
2. [State to verify]
3. [Command to run to continue]
```

This becomes the "continue from here" marker for future sessions or context restoration.
</step>

<step name="clean-artifacts">
**Clean up temporary artifacts:**

If there are temporary files or notes that were created during the session:
- Consolidate important notes into STATE.md
- Remove truly temporary files
- Archive completed work appropriately
</step>

<step name="checkpoint">
**Create checkpoint commit (optional):**

If there are uncommitted changes that represent a stable state:

```powershell
git add -A
git status
```

Offer to commit with message: `chore: session checkpoint - [brief description]`

This creates a safe restore point.
</step>

<step name="report">
**Report compaction complete:**

```markdown
# Context Compaction Complete

**Checkpoint:** [timestamp]

## Summary Preserved

- [N] decisions documented
- [N] issues tracked
- [N] next steps queued

## STATE.md Updated

The Session Continuity section now contains everything needed to resume.

## Next Session

When starting a new session or after context reset:
1. Run `/gsd:resume-work` to restore full context
2. Or reference `.planning/STATE.md` directly

## Continue Now

Ready to continue with: [next action from summary]
```
</step>
</process>

<when_to_use>
Call `/gsd:compact` when:

1. **Session length**: After 20+ back-and-forth turns
2. **Complexity spike**: Before diving into complex debugging
3. **Topic change**: Before switching to a different part of the project
4. **Performance signs**: If responses seem to lose earlier context
5. **Break point**: Before pausing work for an extended period
6. **Proactively**: When you have a natural stopping point
</when_to_use>

<anti_patterns>
- Don't discard unresolved issues - they must persist
- Don't over-summarize - keep enough detail to resume
- Don't compact mid-task - finish current work first
- Don't lose uncommitted changes - checkpoint them
</anti_patterns>

<success_criteria>
- [ ] Current session context assessed
- [ ] Critical information identified and preserved
- [ ] STATE.md updated with session summary
- [ ] Next steps clearly documented
- [ ] Temporary artifacts cleaned up
- [ ] Checkpoint created if appropriate
</success_criteria>
