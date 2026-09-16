# Architecture Trade-Off Analysis

## Purpose

This document records the three design trade-offs. Final decisions should be aligned with the ADRs.

---

## 1. Edge vs Cloud

### Context

The estate has patchy Wi-Fi, while IoT sensors and monitoring workloads need dependable data collection. Cloud services provide centralized analytics, governance, elastic processing, and managed AI capabilities.

### Option A: Primarily Edge

**Advantages**

- Continues local processing during network interruption.
- Reduces latency for time-sensitive local decisions.
- Can reduce unnecessary data transfer through local filtering.

**Disadvantages**

- Hardware is distributed and harder to patch, support, and secure.
- Local compute and storage are constrained.
- Model, configuration, and certificate lifecycle become more complex.

### Option B: Primarily Cloud

**Advantages**

- Centralized governance, monitoring, deployment, and scaling.
- Easier access to managed analytics and AI services.
- Less processing infrastructure to maintain on the estate.

**Disadvantages**

- Depends on connectivity.
- Network failure may delay telemetry and insight generation.
- Continuous transfer can increase cost and latency.

### Decision

Use a **hybrid edge and cloud architecture**.

- Edge handles MQTT connectivity, validation, local buffering, safe preprocessing, and store-and-forward.
- Azure handles centralized ingestion, operational processing, long-term storage, analytics, AI, governance, and enterprise monitoring.

### Consequences

- The team must manage edge security, patching, certificates, configuration, storage capacity, and replay behavior.
- Cloud consumers must support duplicates and late-arriving events.
- Essential estate workflows require documented offline behavior.

### Validation

- Simulate connectivity loss and recovery.
- Verify local persistence, ordering expectations, deduplication, and replay.
- Test buffer capacity using agreed device count, message rate, and outage assumptions.
- Confirm critical alerts and manual fallbacks with operational stakeholders.

---

## 2. Event-Driven vs Request-Response

### Context

Visitor transactions need immediate feedback, while telemetry, notifications, analytics, and downstream workflows benefit from decoupled asynchronous processing.

### Option A: Request-Response

**Advantages**

- Immediate outcome for the caller.
- Straightforward user journey and error response.
- Suitable for queries and transactions requiring confirmation.

**Disadvantages**

- Runtime dependencies are coupled.
- Slow or failed dependencies can create cascading failures.
- Long processing chains increase latency.

### Option B: Event-Driven

**Advantages**

- Producers and consumers are loosely coupled.
- Consumers can scale independently.
- Supports telemetry, fan-out, delayed processing, and resilience.

**Disadvantages**

- Introduces eventual consistency.
- Requires idempotency, schema governance, dead-letter handling, and reconciliation.
- End-to-end tracing and troubleshooting are more complex.

### Decision

Use **both patterns according to the interaction**.

- Request-response for ticket search, purchase initiation, payment authorization, identity, and immediate visitor requests.
- Event-driven processing for ticket lifecycle notifications, telemetry, analytics, operational workflows, and downstream integrations.

### Consequences

- User interfaces must represent pending or eventually consistent states clearly.
- Events require identifiers, versions, timestamps, correlation, idempotency, and ownership.
- Monitoring must cover APIs, queues, subscriptions, consumers, retries, and dead-letter messages.

### Validation

- Load-test critical synchronous paths.
- Test timeouts, retries, circuit breakers, and dependency failure.
- Verify duplicate event processing and replay safety.
- Confirm dashboards expose backlog, processing delay, and dead-letter conditions.

---

## 3. Monolith vs Modular Services

### Context

The platform contains several capabilities: ticketing, family passes, visitor profiles, attractions, animal monitoring, recommendations, AI assistance, notifications, and reporting. Immediate decomposition into many independently deployed services may create unnecessary operational complexity.

### Option A: Monolith

**Advantages**

- Simpler initial deployment and local development.
- Fewer network calls and distributed-system failure modes.
- Easier transactional consistency inside one deployment and data boundary.

**Disadvantages**

- Coupled releases as capabilities grow.
- Coarse scaling and fault isolation.
- Ownership boundaries can erode over time.

### Option B: Independently Deployed Services

**Advantages**

- Independent scaling, release, and failure isolation.
- Clear capability and team ownership.
- Technology choices can vary when justified.

**Disadvantages**

- More deployment pipelines, monitoring, networking, and security configuration.
- Distributed data consistency and testing are more complex.
- Additional latency and cost may be introduced.

### Decision

Start with a **modular architecture with clear domain boundaries**. Deploy a capability independently only when justified by scale, risk, ownership, fault isolation, security, or release cadence.

Likely independently scalable boundaries include:

- Visitor-facing ticketing APIs
- IoT ingestion and stream processing
- AI inference and orchestration
- Notification processing
- Analytical workloads

### Consequences

- Module interfaces and data ownership must be explicit from the beginning.
- The team avoids premature microservice fragmentation.
- Extraction requires compatibility, testing, observability, and migration planning.

### Validation

- Review module dependencies and data ownership.
- Measure scaling and release needs by capability.
- Perform failure-mode analysis for critical workloads.
- Revisit deployment boundaries when operational evidence supports extraction.

---

## Decision Summary

| Trade-Off | Direction | Main Reason |
|---|---|---|
| Edge vs Cloud | Hybrid edge and cloud | Patchy connectivity plus need for centralized analytics and AI |
| Event-Driven vs Request-Response | Use both by interaction | Immediate feedback for transactions and decoupling for telemetry and workflows |
| Monolith vs Modular Services | Modular architecture with selective independent deployment | Preserve clear boundaries without premature distributed-system complexity |

## Related Documents

- [HLD README](../README.md)
- [Logical Architecture](../diagrams/logical_architecture.md)
- [End-to-End Architecture](../diagrams/end_to_end_architecture.md)
- [Azure Architecture](../diagrams/azure_architecture.md)
- [Offline Connectivity Scenario](../scenarios/offline_connectivity.md)
