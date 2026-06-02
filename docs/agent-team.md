# Agent team

I am using **GitHub Copilot CLI in a Codespace** to orchestrate a small custom agent team for building Mona's **Project Pulse dashboard**. The orchestration flow is led by the Orchestrator agent, which delegates planning, implementation, and design work to the specialist agents defined in this repository.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the overall workflow, requests a plan, breaks work into phases, delegates tasks to specialists, and checks that the final result fits together. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository, identifies dependencies and edge cases, and produces the implementation plan the Orchestrator uses to sequence the work. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements code-oriented changes, fixes logic, keeps behavior explicit and testable, and can add runnable-app support files when assigned. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Shapes the UI and UX, including layout, accessibility, visual hierarchy, responsive behavior, and dashboard styling for Project Pulse. | `.github/agents/designer.agent.md` |

Together, these agents provide a clear separation of responsibilities: the **Planner** determines the path, the **Coder** builds the implementation, the **Designer** improves the experience, and the **Orchestrator** manages the end-to-end delivery.
