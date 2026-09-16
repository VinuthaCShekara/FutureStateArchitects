# ADR-014: Standardize End-to-End Observability and Correlation

- **Status:** Accepted
- **Decision owners:** Architecture, business capability owner, security, operations, and data/AI owners as applicable
- **Date:** To be assigned on approval
- **Related HLD:** Von Digitalis Estates digital platform
- **Supersedes:** None
- **Superseded by:** None

## Context

Synchronous APIs, asynchronous events, edge devices, streaming, databases, and AI calls must be diagnosable as one business flow.

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

Use Azure Monitor, Application Insights, and a centralized Log Analytics strategy for platform metrics, structured logs, distributed traces, device health, business KPIs, and AI quality signals. Propagate W3C trace context where supported and standard correlation identifiers in APIs and event envelopes. Separate operational telemetry from business audit records.

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

- Service-local logs only
- Rely on vendor platform metrics without application telemetry
- Log complete payloads by default

## Consequences

- Cross-component diagnosis and SLO measurement improve.
- Telemetry volume, retention, sampling, and access drive cost and privacy considerations.
- Teams must own dashboards, alerts, and runbooks.

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

Excess logging can expose data and increase cost, while undersampling can hide failures. Apply classification, redaction, adaptive sampling where appropriate, retention tiers, archive policy, and access controls.

## Implementation and Validation Actions

- [ ] Define mandatory log fields and PII redaction.
- [ ] Create dashboards for ticketing, messaging, device health, animal monitoring, AI quality, and cost.
- [ ] Define actionable alerts with owner, severity, suppression, and runbook.
- [ ] Test trace propagation across API, messages, replay, and AI orchestration.

## Compliance Evidence Required

- Approved architecture and threat model
- NFR and workload assumptions
- Performance and resilience test results
- Security and privacy review
- Operational runbooks and ownership matrix
- Cost model and budget-alert design

## Review Triggers

Review this ADR when business volumes, regulation, connectivity, safety requirements, Azure service capabilities, regional strategy, data classification, or measured production behavior materially changes.
