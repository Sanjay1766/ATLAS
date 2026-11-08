# ADR-001: Use a Modular Monolith Architecture

- Status: Accepted
- Date: 2026-11-09

## Context

ATLAS is a six-month solo engineering project. Its backend needs to handle service management, telemetry processing, incident detection, diagnosis and recovery workflows.

Starting with multiple microservices would introduce unnecessary deployment, networking and maintenance complexity.

## Decision

Use a modular monolith for the main FastAPI backend.

Separate background workers will handle long-running and resource-intensive tasks.

## Consequences

### Advantages

- Easier initial development.
- Simpler debugging.
- Shared database and business logic.
- Clear internal module boundaries.
- Easier local development.

### Disadvantages

- Backend modules share a deployment lifecycle.
- Poor module boundaries could create tight coupling.
- Scaling individual modules independently will be harder.

## Future Considerations

Individual modules may become separate services if actual performance, reliability or scaling requirements justify the additional complexity.
