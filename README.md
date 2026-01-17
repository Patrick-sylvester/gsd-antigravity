<div align="center">

# GET SHIT DONE (Antigravity Edition)

**A light-weight and powerful meta-prompting, context engineering and spec-driven development system for Google Antigravity & Gemini 3 Pro.**

*Ported from the original work by TÂCHES.*

<br>

**Works on Windows (PowerShell) inside Antigravity.**

<br>

*"If you know clearly what you want, this WILL build it for you. No bs."*

<br>

**Powered by Gemini 3 Pro.**

[Installation](#installation) · [Quick Start](#quick-start) · [Commands](#commands) · [Agent Orchestration](#agent-orchestration)

</div>

---

## What is this?

This is a port of the "Get Shit Done" (GSD) system, adapted for **Google Antigravity**. It provides a structured workflow for building complex software using agentic AI.

The core philosophy remains: **Context is King.** GSD gives the agent the exact context it needs (PROJECT, ROADMAP, STATE) to execute high-quality work without getting lost.

---

## Installation

### Option 1: Clone to Your Project (Recommended)

Copy the GSD folders into your project's `.agent/` directory:

```powershell
# From your project root
git clone https://github.com/glittercowboy/get-shit-done.git temp-gsd
Copy-Item -Path "temp-gsd\.agent\*" -Destination ".agent\" -Recurse -Force
Remove-Item -Path "temp-gsd" -Recurse -Force
```

### Option 2: Global Installation

For GSD commands available across all projects:

```powershell
# Copy workflows to global Antigravity location
Copy-Item -Path ".agent\workflows\gsd-*.md" -Destination "$env:USERPROFILE\.gemini\antigravity\workflows\" -Force
Copy-Item -Path ".agent\skills\*" -Destination "$env:USERPROFILE\.gemini\antigravity\skills\" -Recurse -Force
```

### Verify Installation

After installation, type `/gsd-help` in any Antigravity chat. You should see the command reference.

---

## Quick Start

### New Project (Greenfield)

```
/gsd-new-project          # Agent interviews you, creates PROJECT.md
/gsd-create-roadmap       # Generates ROADMAP.md with phases
/gsd-plan-phase 1         # Creates detailed plan for Phase 1
/gsd-execute-plan         # Executes the plan autonomously
```

### Existing Codebase (Brownfield)

```
/gsd-map-codebase         # Analyzes your code, creates docs
/gsd-new-project          # Then proceed as above
```

### Resume Work

```
/gsd-progress             # See where you left off, continue
```

---

## Commands

| Command | What it does |
|---------|--------------|
| `/gsd-new-project` | Initialize project with vision document |
| `/gsd-create-roadmap` | Generate roadmap and state tracking |
| `/gsd-plan-phase [N]` | Plan tasks for a specific phase |
| `/gsd-execute-plan` | Execute a plan autonomously |
| `/gsd-progress` | Check status, route to next action |
| `/gsd-map-codebase` | Analyze existing code |
| `/gsd-compact` | Compact context to prevent degradation |
| `/gsd-help` | Show all available commands |

### Testing & Debugging

| Command | What it does |
|---------|--------------|
| `/gsd-prepare-testing` | Analyze project, generate test workflows |
| `/gsd-start-testing [file]` | Execute tests via browser automation |
| `/gsd-start-debugging [simple\|complex]` | Work through bug backlogs |

*(See `/gsd-help` for the full 25+ command list)*

---

## Agent Orchestration

Understanding when to use different agent contexts is key to effective GSD usage.

### Single Conversation (Default)

For most GSD work, **stay in your main conversation**. The agent maintains context across commands:

```
/gsd-new-project → /gsd-create-roadmap → /gsd-plan-phase 1 → /gsd-execute-plan
```

All these work best in sequence within ONE conversation.

### When Sub-Agents Auto-Spawn

**You don't need to manually spawn sub-agents for:**
- Browser testing (`/gsd-start-testing`) - uses `browser_subagent` automatically
- Some heavy analysis tasks

The agent handles this for you with fresh context windows.

### When to Open New Conversations (Agent Manager)

Use **CTRL+E** (Windows) or **CMD+E** (Mac) to open Agent Manager when:

| Scenario | Why |
|----------|-----|
| **Parallel testing** | Run multiple test suites simultaneously |
| **Long-running tasks** | Keep main conversation responsive |
| **Context is degraded** | Fresh start without losing project state |
| **Different projects** | Keep contexts cleanly separated |

**How to use Agent Manager:**
1. Press CTRL+E to open Agent Manager
2. Click "New Conversation" 
3. Select your workspace
4. Start the new agent with context: "Read `.planning/STATE.md` and continue work on [task]"

### Keeping Context Fresh

After 20+ turns or when responses seem off:

```
/gsd-compact              # Summarizes context to STATE.md
```

This captures critical decisions into persistent state, allowing you to:
- Continue in current conversation with refreshed context
- Start a new conversation that can pick up from STATE.md

### Recommended Orchestration Patterns

**Solo Development (Most Common):**
```
One main conversation → /gsd-compact periodically → Continue
```

**Heavy Testing Phase:**
```
Main agent: /gsd-prepare-testing
New agent (CTRL+E): /gsd-start-testing auth-testing.md
New agent (CTRL+E): /gsd-start-testing dashboard-testing.md
Main agent: Review results, /gsd-start-debugging simple
```

**Long Session with Multiple Milestones:**
```
Session 1: Complete phases 1-3 → /gsd-compact → /gsd-pause-work
Session 2: /gsd-resume-work → Continue phases 4-6
```

---

## Project Structure

When GSD is active, your project will have:

```
your-project/
├── .agent/
│   ├── workflows/     # GSD command definitions
│   ├── skills/        # Testing agent knowledge
│   └── rules/         # Enforced conventions
├── .planning/         # Project state and plans
│   ├── PROJECT.md     # Vision document
│   ├── ROADMAP.md     # Phase breakdown
│   ├── STATE.md       # Current context
│   └── phases/        # Detailed plans
├── .testing/          # Test workflows (after /gsd-prepare-testing)
└── .backlog/          # Bug tracking (simple/ and complex/)
```

---

## Best Practices

1. **Always approve plans** before execution - review PLAN.md files
2. **Run `/gsd-compact`** after extended sessions or before complex work
3. **Use atomic commits** - one commit per completed task
4. **Keep STATE.md updated** - it's your long-term memory
5. **Separate simple/complex bugs** - different agents, different depths

---

## License

MIT License. Original concept and code by [glittercowboy](https://github.com/glittercowboy/get-shit-done).
Ported for use with Google Antigravity.
