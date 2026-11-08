# Component Responsibilities

This document defines the planned responsibilities and boundaries of the major ATLAS components. ATLAS is currently documentation-only; these responsibilities describe the intended architecture, not implemented functionality.

## Component Overview

| Component | Responsibility |
| --- | --- |
| Web Dashboard | Displays monitored applications, deployments, incidents, evidence, diagnostic hypotheses, and recovery recommendations. Provides interfaces for human approval and decision review. |
| API Backend | Handles authenticated requests, input validation, business workflows, persistence, job scheduling, evidence access, and API responses. |
| Telemetry Pipeline | Receives application and infrastructure signals, normalizes metadata, preserves timestamps and provenance, and routes signals to the appropriate observability systems and backend workflows. |
| OpenTelemetry | Provides instrumentation and collection for application traces, metrics, and related context using a consistent telemetry model. |
| Loki | Stores and queries structured application and platform logs for incident investigation. |
| Prometheus | Stores and queries time-series metrics such as error rate, latency, CPU usage, memory usage, and service health. |
| Grafana | Provides operational dashboards and visualization over metrics, logs, and related observability data. |
| Incident Detection Engine | Identifies abnormal conditions and candidate incidents from thresholds, health checks, alerts, and signal changes. |
| Correlation Engine | Connects related logs, metrics, traces, deployments, builds, dependencies, and resource events into a coherent incident context and timeline. |
| Diagnosis Engine | Ranks potential causes from collected evidence, records supporting and conflicting signals, and expresses uncertainty. AI assistance may be introduced here under bounded, inspectable constraints. |
| Recovery Recommendations | Produces proposed recovery actions such as rollback, restart, configuration correction, or escalation, including expected impact and risk information. |
| Human Approval | Requires an authorized person to approve or reject consequential recovery actions. Approval decisions are explicit and auditable. |
| Recovery Worker | Executes only approved recovery actions through controlled interfaces and reports progress, output, and failures. |
| Health Verification | Checks whether the application and its dependencies recover after an action and records the resulting health evidence. |
| PostgreSQL | Stores durable platform data, including projects, deployments, builds, incidents, evidence references, hypotheses, recommendations, approvals, decisions, and audit records. |
| Redis and RQ | Manages queued background jobs and short-lived coordination state. It is not the source of truth for durable platform data. |
| Monitored Applications | Produce the application and runtime signals that ATLAS observes. They remain outside the ATLAS API process and are treated as separate operational subjects. |

## Responsibility Boundaries

### Web Dashboard and API Backend

The web dashboard is a presentation and interaction layer. It should not contain authoritative business rules, directly access databases, or execute recovery operations. The API backend owns authorization, validation, workflow coordination, and the contract exposed to the dashboard.

### Telemetry and Observability Systems

The telemetry pipeline is responsible for ingestion, normalization, routing, and signal context. Loki, Prometheus, OpenTelemetry, and Grafana provide specialized collection, storage, querying, and visualization capabilities. Durable incident relationships and diagnostic conclusions belong to ATLAS rather than to an observability tool alone.

### Detection, Correlation, and Diagnosis

These components represent distinct stages:

1. **Detection** identifies that an abnormal condition may require attention.
2. **Correlation** determines which signals and events belong to the same incident.
3. **Diagnosis** evaluates the correlated evidence and ranks possible causes.

A diagnosis must retain evidence references, timestamps, provenance, and uncertainty so that developers can inspect how it was formed.

### Recommendations, Approval, and Recovery

Recommendations are proposals, not commands. The approval boundary separates analysis from consequential action. The recovery worker must verify authorization and execute only a bounded action that was approved. Every approval, rejection, execution result, and health verification outcome should be recorded for auditability.

### PostgreSQL, Redis, and Workers

PostgreSQL is the durable system of record. Redis and RQ coordinate asynchronous work but may lose transient state without corrupting the authoritative domain history. Workers perform long-running or high-risk operations outside the API request process and communicate results through controlled job and persistence interfaces.

## Security Principles

- Never execute arbitrary monitored-application or repository code inside the API process.
- Treat monitored applications, workers, the Docker engine, and the API as separate trust boundaries.
- Require authentication and authorization for incident data and recovery approval.
- Apply least privilege, resource limits, and explicit allowlists to worker operations.
- Keep secrets out of source control and use secure configuration management.
- Preserve an audit trail for diagnostic conclusions, approval decisions, recovery actions, and verification results.
