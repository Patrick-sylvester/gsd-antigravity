---
name: testing-agent
description: Automated UI testing agent that uses browser automation to test web applications, identify bugs, and categorize them by complexity.
triggers:
  - "test the application"
  - "run automated tests"
  - "check for bugs"
  - "verify the UI works"
---

# Testing Agent Skill

This skill enables automated browser-based testing of web applications. It can analyze a project to generate test workflows, execute those tests using the browser, and categorize discovered bugs for systematic debugging.

## Capabilities

1. **Project Analysis** - Scan project structure to identify testable features
2. **Test Generation** - Create structured test workflow files
3. **Browser Testing** - Execute tests using the Antigravity browser_subagent
4. **Bug Categorization** - Classify bugs as Simple or Complex
5. **Evidence Collection** - Capture screenshots and recordings

## Testing Philosophy

### What Makes a Good Test

A good automated UI test:
- **Is specific**: Tests one feature or flow at a time
- **Is observable**: Has clear pass/fail criteria
- **Is repeatable**: Same inputs produce same results
- **Is documented**: Explains what and why

### Bug Classification Criteria

**Simple Bugs** (assign to simpler agents, <30 min fixes):
- Visual/CSS issues (alignment, colors, spacing)
- Missing or incorrect text content
- Broken links or missing images
- Component rendering issues
- Minor form validation gaps
- Tooltip or hover state problems

**Complex Bugs** (require deeper investigation):
- Logic errors in business rules
- State management issues
- Data persistence problems
- Authentication/authorization failures
- Multi-step workflow breakdowns
- Race conditions or timing issues
- Cross-component interaction bugs
- API integration failures

## Test Workflow Format

Test workflows should follow this structure:

```markdown
# [Feature Name] Testing Workflow

## Overview
Brief description of what this test workflow covers.

## Pre-conditions
- [ ] Application is running on localhost:[port]
- [ ] User is [logged in/logged out]
- [ ] Database is in [known state]

## Test Cases

### TC-001: [Test Case Name]
**Priority**: Critical | High | Medium | Low
**Type**: Functional | Visual | Integration | Edge Case

**Steps**:
1. Navigate to [URL]
2. [Action to perform]
3. [Another action]

**Expected Result**:
- [What should happen]
- [What should be visible]

**Pass Criteria**:
- [ ] [Specific checkable outcome]
- [ ] [Another checkable outcome]

### TC-002: [Next Test Case]
...
```

## Browser Testing Patterns

When using the browser_subagent for testing:

### Navigation
```
Navigate to the login page at http://localhost:3000/login
Verify the page loads and shows the login form
```

### Form Interaction
```
Click on the email input field
Type "test@example.com"
Click on the password field
Type "password123"
Click the login button
Wait for navigation to complete
```

### Verification
```
Check if the dashboard is visible
Look for the welcome message containing the username
Verify the navigation shows the logged-in state
```

### Screenshot Capture
```
Take a screenshot and save it as evidence of the current state
```

## Evidence Collection

For each failed test, collect:

1. **Screenshot** at the point of failure
2. **Browser recording** of the test execution (automatic with browser_subagent)
3. **Console errors** if visible
4. **Network state** if relevant (pending requests, failed calls)

## Integration with GSD Workflows

This skill is invoked by these GSD workflows:

- `/gsd:prepare-testing` - Uses this skill's patterns to generate test workflows
- `/gsd:start-testing` - Uses this skill to execute tests
- `/gsd:start-debugging` - References bug classification criteria

## File Locations

When active, this skill expects:

```
[project-root]/
├── .testing/           # Test workflow files
│   ├── README.md
│   └── [feature]-testing.md
├── .backlog/           # Bug reports
│   ├── simple/
│   └── complex/
└── .planning/          # Project context (for analysis)
```

## Best Practices

### For Test Generation
1. Start with user-facing features (what users actually do)
2. Cover the "happy path" first, then edge cases
3. Group related tests into single workflow files
4. Keep each test case focused on one verification

### For Test Execution
1. Always verify pre-conditions before starting
2. Take screenshots at key decision points
3. Don't skip tests - mark as "blocked" if can't run
4. Record exact steps that led to failure

### For Bug Reporting
1. Be specific about reproduction steps
2. Include evidence (screenshots, recordings)
3. Suggest likely file locations when possible
4. Classify honestly - don't downplay complexity
