# Development Guidelines

## Philosophy

Build ATLAS in small, verifiable increments. Prefer clear module boundaries, explicit interfaces, simple designs, and documented technical reasoning. Do not add application components, dependencies, or infrastructure before the relevant milestone requires them.

## Engineering Standards

- Use clean, modular code with explicit separation of responsibilities.
- Use Python type hints and validate API inputs.
- Manage database changes through migrations.
- Add automated tests for behavior and important failure paths.
- Use structured logging and secure configuration management.
- Keep secrets in environment-backed configuration.
- Maintain meaningful documentation and reproducible development environments.
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