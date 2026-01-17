---
name: gsd-start-testing
description: Execute a testing workflow using browser-based UI testing
argument-hint: "<testing-file.md> - e.g., auth-testing.md"
---


> [!IMPORTANT]
> **System Note**: This workflow uses the Antigravity browser_subagent for UI testing. All browser interactions are recorded and saved as artifacts.


<objective>
Execute a specific test workflow from the `.testing/` folder using browser automation.

This is the EXECUTION phase of testing. It:
1. Loads the specified test workflow
2. Verifies the dev server is running
3. Executes each test case using the browser
4. Captures evidence (screenshots, recordings)
5. Logs failures to `.backlog/simple/` or `.backlog/complex/`

Output: Test results summary, bug reports for failures
</objective>

<execution_context>
@C:/Users/jdash/Documents/GSD Google Antigravity/get-shit-done/skills/testing-agent/SKILL.md
@C:/Users/jdash/Documents/GSD Google Antigravity/get-shit-done/skills/testing-agent/templates/bug-report.md
</execution_context>

<context>
Required: $ARGUMENTS (test workflow filename)
- e.g., "auth-testing.md" or ".testing/auth-testing.md"

**Load the test workflow:**
@.testing/$ARGUMENTS (or full path if provided)
</context>

<process>
<step name="validate">
**Validate inputs:**

1. Check that $ARGUMENTS is provided
   - If not: List available test workflows in `.testing/` and ask user to choose

2. Verify the test file exists
   - If not: Show error and list available files

3. Read and parse the test workflow file
   - Extract pre-conditions
   - Count test cases
   - Identify test order
</step>

<step name="preflight">
**Pre-flight checks:**

1. Ensure `.backlog/simple/` and `.backlog/complex/` exist
2. Determine the application URL from test pre-conditions
3. Check if dev server appears to be running:

```powershell
# Try to reach the dev server
try {
    $response = Invoke-WebRequest -Uri "http://localhost:3000" -TimeoutSec 5 -UseBasicParsing
    Write-Host "Dev server is responding"
} catch {
    Write-Host "Warning: Dev server may not be running"
}
```

If server not running:
- Warn the user
- Offer to start it or wait for them to start it
- Don't proceed until server is accessible
</step>

<step name="execute">
**Execute test cases:**

For each test case in the workflow:

1. **Announce the test**: Display test ID and name
2. **Check pre-conditions**: Ensure any test-specific setup is done
3. **Execute via browser_subagent**:

Use the browser_subagent tool to:
- Navigate to the required URL
- Perform the documented steps
- Observe the actual results
- Take screenshots at key points
- Compare against expected results

Example browser_subagent task:
```
Navigate to http://localhost:3000/login
Enter "test@example.com" in the email field
Enter "TestPass123!" in the password field
Click the "Sign In" button
Wait for navigation to complete
Check if the URL is now /dashboard
Look for a welcome message containing the username
Take a screenshot showing the current state
Report: PASS if on dashboard with welcome message, FAIL otherwise with details
```

4. **Record result**: Pass, Fail, Partial, or Blocked
5. **If failed**: Proceed to log-bug step for this test
</step>

<step name="log-bug">
**Log bug for failed test:**

For each failed/partial test:

1. **Classify the bug**:
   - Is it visual/CSS? → Simple
   - Is it broken link or missing text? → Simple
   - Is it a logic error? → Complex
   - Does it involve state or data? → Complex
   - Is it a multi-step failure? → Complex

2. **Generate bug ID**:
   - Count existing bugs in `.backlog/simple/` and `.backlog/complex/`
   - New ID = BUG-[NNN] where NNN is next available number

3. **Create bug report**:
   - Use template from `bug-report.md`
   - Fill in all fields from test execution
   - Include screenshot path
   - Note the test case that found it

4. **Save bug report**:
   - Simple: `.backlog/simple/BUG-[NNN]-[slug].md`
   - Complex: `.backlog/complex/BUG-[NNN]-[slug].md`

5. **Continue to next test** (don't stop on failures)
</step>

<step name="summary">
**Present test results:**

```markdown
# Test Results: [Test Workflow Name]

**Run Date**: [date/time]
**Duration**: [approximate time]

## Summary

| Status | Count |
|--------|-------|
| ✅ Passed | [N] |
| ❌ Failed | [N] |
| ⚠️ Partial | [N] |
| ⏭️ Blocked | [N] |
| **Total** | [N] |

## Pass Rate: [X]%

## Bugs Logged

### Simple Bugs ([N])
- BUG-001: [title] → `.backlog/simple/BUG-001-slug.md`
- ...

### Complex Bugs ([N])
- BUG-002: [title] → `.backlog/complex/BUG-002-slug.md`
- ...

## Next Steps

[Based on results, recommend:]
- If all passed: "All tests passed! Consider running another test workflow."
- If bugs found: "Run `/gsd:start-debugging simple` to address simple bugs first."
```
</step>

<step name="offer">
**Offer next actions:**

Based on results:
- If bugs in simple: Suggest `/gsd:start-debugging simple`
- If bugs in complex: Suggest `/gsd:start-debugging complex`
- If all passed: Suggest next test workflow or completion
- Always offer to run another test workflow
</step>
</process>

<anti_patterns>
- Don't skip tests - mark as "blocked" if truly can't run
- Don't make up results - actually execute in browser
- Don't stop on first failure - complete all tests
- Don't create partial bug reports - fill all required fields
- Don't guess bug classification - use the criteria in SKILL.md
</anti_patterns>

<success_criteria>
- [ ] All test cases in workflow were attempted
- [ ] Each test properly executed via browser
- [ ] Screenshots captured for failures
- [ ] Bug reports created for all failures
- [ ] Bugs correctly classified as simple/complex
- [ ] Summary presented with accurate counts
- [ ] User knows next steps
</success_criteria>
