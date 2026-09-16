# ADR-009: Use Edge Store-and-Forward with Idempotent Cloud Replay

- **Status:** Accepted
- **Decision owners:** Architecture, business capability owner, security, operations, and data/AI owners as applicable
- **Date:** To be assigned on approval
- **Related HLD:** Von Digitalis Estates digital platform
- **Supersedes:** None
- **Superseded by:** None

## Context

Field devices must continue capturing operational and animal-monitoring observations when network connectivity is unstable.

The design must support growth from approximately 5,000 to at least 15,000 visitors per day, 40 rides, more than 200 animals across 55 displays and enclosures, intermittent estate connectivity, and business-critical ticketing and animal-care workflows. Final regions, SKUs, numeric SLOs, and cost commitments remain subject to non-functional requirements, load testing, compliance review, and cost analysis.

## Decision Drivers

- Business continuity and visitor safety
- Animal-welfare safeguards
- Scalability and operational supportability
- Security, privacy, and auditability
- Loose coupling and replaceability
- Measurable reliability, performance, and cost
- Avoidance of unvalidated product, capacity, and cost commitments

## Decision

Persist validated device events in a durable local buffer before cloud acknowledgement. Assign globally unique event IDs and capture device timestamp, gateway receive timestamp, schema version, sequence metadata where needed, and priority. Replay securely after recovery. Cloud consumers must deduplicate and handle late or out-of-order events.

## Decision Flow

```mermaid
flowchart LR
    A[Business or device interaction] --> B[Governed platform boundary]
    B --> C[Decision-specific processing]
    C --> D[Durable state or event]
    D --> E[Observability and operational control]
    E --> F[Business outcome]
```

> The diagram is a decision-level view. Detailed component and sequence diagrams remain in the HLD scenario documentation.

## Alternatives Considered

- Drop events when offline
- Hold events only in memory
- Assume exactly-once transport

## Consequences

- Connectivity interruptions do not automatically cause data loss.
- Local storage must be sized, encrypted, monitored, and purged safely.
- Replay bursts can increase cloud load and processing delay.

### Positive

- Establishes an explicit design standard that downstream solution designs can validate against.
- Supports consistent security, observability, resilience, and governance controls.

### Negative or Trade-offs

- Introduces implementation and operational work that must be reflected in delivery plans.
- Requires evidence from NFR definition, performance testing, security review, and cost analysis before production approval.

## Security and Privacy Implications

- Apply least privilege, encryption in transit and at rest, managed identities where supported, and Key Vault for secrets and certificates.
- Minimize personal and sensitive data in payloads, logs, traces, prompts, and analytical stores.
- Record privileged and material business actions in tamper-resistant audit records.
- Perform threat modelling and privacy review before production release.

## Reliability and Failure Handling

- Use bounded retries, timeout budgets, circuit breakers, bulkheads, idempotency, and dead-letter handling where relevant.
- Define deterministic degradation behavior for dependency, connectivity, and AI failures.
- Validate backup, restore, replay, and reconciliation procedures.

## Observability

- Emit structured logs, platform and application metrics, distributed traces, business KPIs, and audit events.
- Propagate correlation and trace identifiers across API, messaging, streaming, edge, and AI flows.
- Define service ownership, alert severity, runbook, and escalation path for each actionable condition.

## Cost Implications

No numeric estimate is approved by this ADR. Cost drivers include service tier and capacity, transaction and event volume, data retention, network transfer, edge hardware, observability ingestion, AI inference, and recovery topology. Validate the decision using agreed workload assumptions and the Azure pricing process before approval.

## Risks and Mitigations

Gateway theft, disk corruption, and unbounded backlog are mitigated with encryption, secure boot where supported, tamper controls, checksums, quotas, priority shedding, and health alerts.

## Implementation and Validation Actions

- [ ] Define buffer duration by event class and business criticality.
- [ ] Define disk-pressure prioritization and loss reporting.
- [ ] Throttle replay and isolate live from replay traffic where required.
- [ ] Test disconnect, reconnect, clock drift, duplicate, corruption, and full-disk scenarios.

## Compliance Evidence Required

- Approved architecture and threat model
- NFR and workload assumptions
- Performance and resilience test results
- Security and privacy review
- Operational runbooks and ownership matrix
- Cost model and budget-alert design

## Review Triggers

Review this ADR when business volumes, regulation, connectivity, safety requirements, Azure service capabilities, regional strategy, data classification, or measured production behavior materially changes.
