# Agent team

The Mona's Project Pulse dashboard team is coordinated through GitHub Copilot CLI in a Codespace:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 5.5 (copilot) | Coordinates the specialist agents, breaks work into phases, manages dependencies and file ownership, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 5.5 (copilot) | Researches the repository and requirements, identifies risks and edge cases, and produces an ordered implementation plan without writing code. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements assigned code, fixes bugs, adds runnable-app support when required, and validates deterministic, testable behavior. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.7 Flash (copilot) | Defines and implements UI/UX work focused on usability, accessibility, information hierarchy, interaction flow, responsive behavior, and polished Project Pulse visuals. | `.github/agents/designer.agent.md` |

The Orchestrator uses the Planner first, then assigns non-overlapping implementation and design work to the Coder and Designer, running phases in parallel only when their scopes and dependencies allow it.
