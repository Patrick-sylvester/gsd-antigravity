---
activation: always
---

# Git Commit Standards

## Atomic Commits

Make one commit per completed task. Each commit should be:
- **Focused**: One logical change
- **Complete**: Includes all related files
- **Testable**: Can be reverted independently

## Commit Message Format

```
type(scope): brief description

[optional body with more detail]
```

### Types
- `feat`: New feature
- `fix`: Bug fix  
- `docs`: Documentation
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance

### Scope
Use the phase/plan number or component name:
- `feat(03-02): add user authentication`
- `fix(BUG-001): correct login validation`

## When to Commit

1. After each PLAN.md task is verified complete
2. After fixing each bug from backlog
3. Before switching to different work

## Never Commit

- Incomplete work without a clear continuation path
- Multiple unrelated changes in one commit
- Secrets, credentials, or sensitive data
