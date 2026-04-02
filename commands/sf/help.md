---
name: sf:help
description: List all SF Consulting Framework commands
allowed-tools:
  - Read
---
<objective>
Display the SF Consulting Framework command reference.

Output ONLY the reference content below. Do NOT add:
- Project-specific analysis
- Git status or file context
- Next-step suggestions
- Any commentary beyond the reference
</objective>

<process>
Output the following command reference exactly:

# SF Consulting Framework Commands

These commands are shortcuts for the most common workflows. You can also trigger any workflow with natural language (e.g., "raise a question", "what's next").

## Knowledge Base
| Command | What it does |
|---------|-------------|
| `/sf:ask` | Raise a new question in the knowledge base |
| `/sf:answer` | Record an answer to an open question |

## Information Processing
| Command | What it does |
|---------|-------------|
| `/sf:process` | Process a transcript or brain dump |

## Planning and Structure
| Command | What it does |
|---------|-------------|
| `/sf:init` | Scaffold a new consulting engagement |
| `/sf:tickets` | Create or manage Linear tickets |

## Reporting and Status
| Command | What it does |
|---------|-------------|
| `/sf:briefing` | Morning briefing: project status + recommended focus |
| `/sf:status` | Generate a client-facing status report |
| `/sf:wrap-up` | End-of-session summary and log |

## Health and Governance
| Command | What it does |
|---------|-------------|
| `/sf:risks` | Project health and risk scan |
| `/sf:prep` | Generate a meeting preparation brief |

## Additional Workflows (use natural language)

These workflows are available via natural language triggers in the SF Consulting Framework skill:

| Trigger phrase | What it does |
|---------------|-------------|
| "capture requirement" | Record a business requirement |
| "decision made" | Record a design decision |
| "record meeting" / "meeting notes" | Process meeting notes |
| "sync with Linear" | Sync execution plan with Linear |
| "follow ups" / "what am I waiting on" | Show pending follow-ups |
| "scope check" / "scope creep" | Check for scope drift |
| "deploy checklist" / "go/no-go" | Deployment readiness checklist |
| "test plan" / "UAT plan" | Generate test/UAT plan |
| "review decisions" / "decision audit" | Audit decisions for consistency |
| "handoff" / "onboarding doc" | Generate handoff document |
| "add epic" / "scaffold epic" | Scaffold a new epic directory |
| "add feature" | Scaffold a feature within an epic |
</process>
