# ADR-002: Use Azure IoT Hub for Managed Device Connectivity and Telemetry Ingestion

- **Status:** Accepted
- **Decision owners:** Architecture, business capability owner, security, operations, and data/AI owners as applicable
- **Date:** To be assigned on approval
- **Related HLD:** Von Digitalis Estates digital platform
- **Supersedes:** None
- **Superseded by:** None

## Context

Sensors, scanners, counters, and approved cameras need authenticated connectivity, device lifecycle controls, bidirectional management, and scalable telemetry ingestion.

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

Use Azure IoT Hub as the primary cloud ingress for managed IoT devices and edge gateways. Use MQTT where supported. Give each device or gateway a unique identity and prefer certificate-based authentication. Route telemetry to downstream stream-processing and storage services.

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

- Connect devices directly to Event Hubs
- Use a custom MQTT broker as the primary cloud ingress
- Send telemetry through general-purpose REST APIs

## Consequences

- Device identity and ingestion are standardized.
- IoT Hub capacity, message size, routing, and quota assumptions must be validated.
- Device provisioning and certificate lifecycle become mandatory operational capabilities.

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

Compromised or misconfigured devices could inject invalid data. Apply per-device identity, revocation, schema validation, rate controls, and device-health alerting.

## Implementation and Validation Actions

- [ ] Select the provisioning approach and ownership model.
- [ ] Define device twins or equivalent configuration boundaries.
- [ ] Define telemetry schemas, routing rules, priority classes, and quarantine handling.
- [ ] Load-test using projected device counts, frequencies, payload sizes, and reconnect bursts.

## Compliance Evidence Required

- Approved architecture and threat model
- NFR and workload assumptions
- Performance and resilience test results
- Security and privacy review
- Operational runbooks and ownership matrix
- Cost model and budget-alert design

## Review Triggers

Review this ADR when business volumes, regulation, connectivity, safety requirements, Azure service capabilities, regional strategy, data classification, or measured production behavior materially changes.
