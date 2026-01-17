---
activation: always
---

# GSD Conventions

When working in a project with a `.planning/` folder, you are in a GSD-managed project. Follow these conventions:

## State Management

1. **Always read STATE.md first** when starting or resuming work
2. **Update STATE.md after completing tasks** with key decisions and progress
3. **Never modify completed SUMMARY.md files** - they are historical records

## Plan Execution

1. **Follow PLAN.md task structure exactly** - don't skip or reorder tasks
2. **Complete verification steps** before marking tasks done
3. **Create SUMMARY.md after plan completion** with what was accomplished

## File Organization

1. All planning artifacts go in `.planning/`
2. Test workflows go in `.testing/`
3. Bug reports go in `.backlog/simple/` or `.backlog/complex/`

## When resuming work

If you see a `.planning/` folder, suggest `/gsd:progress` to check current state before starting new work.
