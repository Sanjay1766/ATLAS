# Architecture Decisions

Architectural decisions will be recorded as small, reviewable documents when they affect system boundaries, security, data ownership, operational behavior, or long-term maintainability.

## ADR Format

Each decision should contain:

- **Decision ID**
- **Title**
- **Status**
- **Context**
- **Decision**
- **Alternatives considered**
- **Consequences**

Decisions should explain the technical reasoning, identify meaningful tradeoffs, and be updated when the decision is superseded.

## ADR-001: Use a Modular Monolith with Separate Workers

- **Decision ID:** ADR-001
- **Title:** Use a modular monolith with separate workers
- **Status:** Accepted
- **Context:** ATLAS needs clear domain boundaries and independent execution for long-running build, deployment observation, monitoring, and incident-analysis operations. The project is initially developed by one person and does not need the operational overhead of many independently deployed services.
- **Decision:** Start with a modular monolith for the API and run build, deployment, monitoring, and incident-analysis workers as separate processes connected through Redis-backed jobs. Keep domain boundaries explicit inside the backend and require human approval for consequential recovery actions.
- **Alternatives considered:** A single-process application would mix request handling with high-risk long-running work. A microservice architecture would add deployment, networking, and observability overhead before scale or ownership boundaries justify it.
- **Consequences:** The initial system remains easier to develop and operate while workers provide execution isolation and independent scaling. Module contracts must remain explicit so that later extraction is possible if it becomes technically justified.