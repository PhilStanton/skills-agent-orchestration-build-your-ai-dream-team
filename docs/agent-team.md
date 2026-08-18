# Agent team

This project uses a small custom agent team to build Mona's Project Pulse dashboard. Each agent is defined in .github/agents/*.agent.md and is orchestrated from a Codespace using the GitHub Copilot CLI.

- Designer — model: Gemini 3.1 Pro (copilot)
  - Responsibility: UI/UX, accessibility, information architecture, interaction flow, and visual design for the dashboard.
  - Definition: .github/agents/designer.agent.md

- Orchestrator — model: Claude Opus 4.7 (copilot)
  - Responsibility: Breaks the work into phases, delegates tasks to specialists, coordinates parallelism and sequencing, and verifies integration.
  - Definition: .github/agents/orchestrator.agent.md

- Coder — model: GPT-5.5 (copilot)
  - Responsibility: Implement code, create runnable app support, and validate changes within assigned file scopes.
  - Definition: .github/agents/coder.agent.md

- Planner — model: Claude Opus 4.7 (copilot)
  - Responsibility: Research the codebase, produce implementation plans, list file assignments, risks, and validation steps.
  - Definition: .github/agents/planner.agent.md

Note: The GitHub Copilot CLI running inside this Codespace is used to orchestrate these agents and to perform edits, tests, and validations as directed by the Orchestrator.