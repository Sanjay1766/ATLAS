# Development Guidelines

## Philosophy

Build ATLAS in small, verifiable increments. Prefer clear module boundaries, explicit interfaces, simple designs, and documented technical reasoning. Do not add application components, dependencies, or infrastructure before the relevant milestone requires them.

## Engineering Standards

ATLAS development follows these project-wide principles:

1. **Python code must be modular and testable.** Modules should have clear responsibilities, explicit interfaces, and limited coupling.
2. **Every major feature must have automated tests.** Tests should cover expected behavior, important failure paths, and relevant integration boundaries.
3. **APIs must be versioned.** Public API changes must use an intentional compatibility and migration strategy rather than silently breaking existing clients.
4. **Errors must be logged with useful context.** Logs should include sufficient structured context to support diagnosis without exposing secrets or sensitive data.
5. **Secrets must never be committed to Git.** Credentials, tokens, private keys, and environment-specific secrets must be supplied through secure configuration mechanisms.
6. **Recovery operations must be auditable.** Recommendations, approvals, executions, outcomes, and acting identities must be recorded for later review.
7. **AI must provide evidence for its recommendations.** Diagnostic conclusions and recovery suggestions must reference the signals, events, and uncertainty that support them.
8. **Destructive operations require human approval.** The system must not execute consequential recovery actions autonomously by default.
9. **Documentation must evolve alongside implementation.** Public interfaces, architecture, operational behavior, security assumptions, and decisions must be updated with the code they describe.

Additional standards:

- Use Python type hints and validate API inputs.
- Manage database changes through migrations.
- Use structured logging and secure configuration management.
- Maintain reproducible development environments.
- Avoid unnecessary dependencies, abstractions, and premature microservices.

## Testing Expectations

New behavior should have focused automated tests appropriate to its risk. Changes that cross module or infrastructure boundaries should include integration coverage where practical. Tests should be deterministic, readable, and runnable in a clean environment.

## Documentation Expectations

Document public interfaces, operational assumptions, security boundaries, and decisions that affect future work. Keep planned, in-progress, and implemented behavior clearly distinguished.

## Branch and Commit Conventions

Use short-lived branches named for the work they contain, such as `docs/architecture-baseline` or `feature/build-records`. Commits should be small, focused, and written in the imperative mood. Do not commit generated files, secrets, or unrelated changes.

The repository owner controls commits and publishing. No automated process should create commits or push changes.

## Local Setup

Application setup commands will be documented when the corresponding application components exist. During initialization, this repository intentionally contains documentation only and has no frontend, backend, worker, or infrastructure setup to run.