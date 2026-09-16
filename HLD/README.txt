# High-Level Design (HLD) Documentation

This directory contains the high-level design documentation for the **Von Digitalis Estates digital platform**. It describes the end-to-end architecture for digital ticketing, visitor insights, estate operations, IoT-enabled animal monitoring, and AI-assisted visitor services.

The HLD focuses on how business capabilities, applications, Azure services, data platforms, IoT devices, and AI components interact to support the estate's operational and growth objectives.

---

## Overview

Von Digitalis Estates currently receives approximately **5,000 visitors per day** and expects this number to grow to at least **15,000 visitors per day within three years**. The estate includes **40 amusement rides** and more than **200 animals across 55 displays and enclosures**.

The proposed architecture supports the following business goals:

- Enable visitors to purchase individual tickets and family passes.
- Understand the popularity of rides, exhibits, and other areas of the estate.
- Improve visitor engagement and encourage repeat visits.
- Monitor animal health, feeding patterns, and population levels.
- Support operations despite patchy Wi-Fi coverage across the estate.
- Provide a scalable foundation for analytics and AI-assisted services.

Each scenario document should include:

- **Business Context:** Why the scenario matters.
- **Actors:** Users, systems, devices, and services involved.
- **Sequence Diagram:** Visual representation of interactions.
- **Data Flow:** How data moves through the system.
- **Error Handling:** Expected behavior when a dependency or connection fails.
- **Performance Considerations:** Latency, throughput, availability, and scalability.
- **Security Considerations:** Identity, access, data protection, and device security.
- **Cost Considerations:** Main resources and cost drivers, without using unvalidated estimates.

---

## Architecture Views

The HLD contains the following architecture views:

1. **[Context Diagram](diagrams/context_diagram.md)**  
   Shows visitors, estate staff, animal care teams, IoT devices, external systems, and the Von Digitalis digital platform.

2. **[Logical Architecture](diagrams/logical_architecture.md)**  
   Shows the experience, API, business service, integration, data, AI, IoT, security, and observability layers.

3. **[End-to-End Solution Architecture](diagrams/end_to_end_architecture.md)**  
   Shows how applications, services, events, data stores, AI capabilities, and operational tools interact.

4. **[Azure Architecture](diagrams/azure_architecture.md)**  
   Maps the logical components to the proposed Azure services.

5. **[Data Flow View](diagrams/data_flow.md)**  
   Shows synchronous, event-driven, streaming, batch, and AI inference data flows.

---

## Scenarios

### 1. [Ticket Purchase and Family Pass](scenarios/ticket_purchase.md)

End-to-end visitor journey from selecting a ticket or family pass to payment and ticket confirmation.

**Key Components:**

- Web or mobile visitor channel
- Azure Front Door
- Azure API Management
- Ticketing Service
- Family Pass Service
- Payment Provider
- Azure SQL Database
- Azure Service Bus
- Notification Service

**Flow:** Visitor selects ticket or family pass → API validates request → Ticketing Service creates order → Payment is authorized → Ticket is issued → Confirmation is sent

---

### 2. [Visitor Popularity and Crowd Insights](scenarios/visitor_insights.md)

Collection and analysis of visitor movement and attraction usage to understand which parts of the estate are most popular.

**Key Components:**

- Entry scanners and attraction counters
- MQTT-capable devices
- Edge Gateway
- Azure IoT Hub
- Azure Event Hubs
- Stream Processing
- Azure Data Lake Storage
- Analytics and reporting platform

**Flow:** Visitor interaction → Device or scanner event → Edge Gateway → IoT ingestion → Stream processing → Data platform → Popularity dashboard

---

### 3. [Animal Health Monitoring](scenarios/animal_health_monitoring.md)

Continuous monitoring of animal health and enclosure conditions using sensor observations, historical data, and anomaly detection.

**Key Components:**

- Animal and enclosure sensors
- Cameras where approved
- MQTT-capable devices
- Edge Gateway with local buffering
- Azure IoT Hub
- Azure Functions or stream processing
- Time-series and historical data storage
- AI or machine-learning inference service
- Alerting and animal-care workflow

**Flow:** Sensor or image observation → Edge validation and buffering → Cloud ingestion → Data processing → Anomaly detection → Human review → Animal-care action

---

### 4. [Feeding and Population Monitoring](scenarios/feeding_population_monitoring.md)

Monitoring feeding behavior, food consumption, and population levels for selected animals, including the jumping piranha collection.

**Key Components:**

- Feeding sensors and observation devices
- Cameras where approved
- Edge Gateway
- Azure IoT Hub
- Event processing service
- Azure AI Vision or approved machine-learning model
- Operational data store
- Animal-care dashboard
- Human validation workflow

**Flow:** Feeding or population observation → Edge capture → Cloud ingestion → AI-assisted analysis → Confidence check → Human validation → Dashboard or alert

---

### 5. [Personalized Visitor Recommendations](scenarios/personalized_recommendations.md)

Providing relevant attraction and activity recommendations using visitor preferences, ticket information, live estate conditions, and approved content.

**Key Components:**

- Web or mobile visitor channel
- Azure API Management
- Visitor Profile Service
- Recommendation Service
- Azure Machine Learning or approved recommendation model
- Operational data store
- Cache
- Feedback and analytics pipeline

**Flow:** Visitor request → Profile and context retrieval → Recommendation inference → Rules and safety checks → Personalized suggestions → Feedback capture

---

### 6. [AI Visitor Assistant](scenarios/ai_visitor_assistant.md)

A conversational assistant that helps visitors with estate information, attraction discovery, itinerary planning, and supported operational questions.

**Key Components:**

- Web or mobile chat interface
- Azure API Management
- AI Orchestration Service
- Azure OpenAI
- Azure AI Search
- Approved estate knowledge base
- Content safety controls
- Human escalation path
- Monitoring and feedback store

**Flow:** Visitor question → Authentication or anonymous session checks → AI orchestration → Knowledge retrieval → Grounded response generation → Safety validation → Response and feedback

---

### 7. [Offline and Intermittent Connectivity](scenarios/offline_connectivity.md)

Maintaining essential estate operations when Wi-Fi or external connectivity is unavailable or unstable.

**Key Components:**

- MQTT-capable field devices
- Edge Gateway
- Local store-and-forward buffer
- Device health monitor
- Azure IoT Hub
- Retry and reconciliation service
- Operational alerting

**Flow:** Device event → Local validation → Local buffer → Connectivity check → Secure replay → Cloud ingestion → Deduplication and reconciliation

---

## Architecture Principles

### 1. Event-Driven Architecture

- Use asynchronous events for IoT telemetry, ticket lifecycle events, notifications, analytics, and downstream processing.
- Reduce direct dependencies between producers and consumers.
- Scale event consumers independently.
- Apply idempotency, retry, and dead-letter handling.

### 2. Edge and Cloud Architecture

- Process connectivity-sensitive and time-sensitive activities near the estate devices.
- Buffer events locally during network interruptions.
- Use cloud services for centralized data processing, analytics, AI, governance, and long-term storage.
- Avoid making essential on-site functions dependent on continuous cloud connectivity.

### 3. Modular Services

- Separate ticketing, visitor profiles, recommendations, animal monitoring, notifications, and reporting into clear business capabilities.
- Define ownership and interfaces for each service.
- Support independent deployment and scaling where justified.
- Avoid unnecessary service fragmentation.

### 4. API-First Integration

- Expose governed interfaces through Azure API Management.
- Apply consistent authentication, authorization, throttling, versioning, and observability.
- Use synchronous APIs only where an immediate response is required.

### 5. Data Lakehouse and Governed Data Products

- Retain raw data for traceability where permitted.
- Clean, validate, classify, and enrich trusted data.
- Publish business-ready datasets for reporting, analytics, and approved AI use cases.
- Apply retention, privacy, quality, and lineage controls.

### 6. Responsible and Replaceable AI

- Ground generative AI responses in approved estate information.
- Use confidence thresholds and human validation for animal welfare decisions.
- Monitor quality, groundedness, drift, latency, safety, and cost.
- Place AI services behind an abstraction or orchestration layer to reduce provider lock-in.
- Provide deterministic fallback behavior when AI is unavailable or unsuitable.

### 7. Security by Design

- Apply Zero Trust principles across users, APIs, workloads, data, and devices.
- Use least-privilege access and managed identities where supported.
- Encrypt data in transit and at rest.
- Store secrets and certificates in Azure Key Vault.
- Treat IoT device identity and lifecycle management as first-class security concerns.

### 8. Observability and Operational Readiness

- Collect centralized metrics, logs, traces, device health, and AI quality signals.
- Define actionable alerts and operational ownership.
- Use correlation identifiers across synchronous and asynchronous flows.
- Design for graceful degradation and recoverability.

---

## Data Flow Patterns

### Pattern 1: Request-Response (Synchronous)

```text
Visitor Channel → Azure Front Door → API Management → Business Service → Database or Provider → Response
```

**Use Cases:** Ticket search, ticket purchase, visitor profile lookup, itinerary request  
**Consistency:** Strong consistency for payment and confirmed ticket transactions  
**Design Focus:** Authentication, authorization, timeout, idempotency, and user-friendly errors

---

### Pattern 2: Event Messaging (Asynchronous)

```text
Producer → Service Bus or Event Grid → Consumer(s) → Database, Notification, Analytics, or Workflow
```

**Use Cases:** Ticket-created events, payment outcomes, notifications, operational workflows  
**Consistency:** Eventual consistency  
**Design Focus:** Idempotent consumers, retries, dead-letter queues, schema versioning, and traceability

---

### Pattern 3: IoT Telemetry Streaming

```text
Sensor or Camera → Edge Gateway → Azure IoT Hub → Stream Processing → Operational Store and Data Lake → Analytics or AI
```

**Use Cases:** Enclosure conditions, feeding observations, population observations, device health  
**Consistency:** Eventual consistency  
**Design Focus:** Store-and-forward, device identity, message ordering where required, deduplication, and late-arriving events

---

### Pattern 4: Batch Analytics and Model Training

```text
Data Lake Raw Zone → Validation and Transformation → Curated Data → Analytics or Model Training → Model Registry
```

**Use Cases:** Historical popularity analysis, trend reporting, model training, business reporting  
**Consistency:** Batch-window consistency  
**Design Focus:** Data quality, lineage, reproducibility, privacy, and controlled model promotion

---

### Pattern 5: Real-Time AI Inference

```text
Service → Feature and Context Retrieval → Model Endpoint → Policy Validation → Prediction → Service Response
```

**Use Cases:** Recommendations, crowd prediction, animal-health anomaly detection  
**Consistency:** Point-in-time context with non-deterministic model output where applicable  
**Design Focus:** Confidence thresholds, model versioning, fallback rules, performance, and outcome monitoring

---

### Pattern 6: Retrieval-Augmented Generation

```text
Visitor → AI Assistant → Orchestration → Azure AI Search → Azure OpenAI → Safety and Groundedness Checks → Response
```

**Use Cases:** Estate information, attraction discovery, itinerary assistance  
**Consistency:** Non-deterministic generated response grounded in approved content  
**Design Focus:** Source grounding, content safety, prompt protection, evaluation, human escalation, and feedback

---

## Component Inventory

| Component | Type | Purpose | Scaling Approach |
|---|---|---|---|
| **Azure Front Door** | Edge entry service | Global entry point, routing, and web protection | Managed scaling |
| **Azure API Management** | API gateway | API routing, policy enforcement, throttling, and versioning | Scale by service tier and capacity |
| **Azure App Service / Container Apps** | Application runtime | Hosts visitor and business services | Horizontal scaling |
| **Azure Functions** | Serverless compute | Event processing and lightweight workflows | Event-driven scaling |
| **Azure Service Bus** | Enterprise messaging | Commands, queues, topics, and dead-letter handling | Scale by tier and messaging units |
| **Azure Event Grid** | Event distribution | Routes domain and platform events | Managed scaling |
| **Azure IoT Hub** | IoT ingestion | Secure device connectivity and telemetry ingestion | Scale by IoT Hub units and tier |
| **Edge Gateway** | Edge runtime | MQTT connectivity, local processing, and store-and-forward | Scale by estate zone and workload |
| **Azure SQL Database** | Relational database | Ticket, order, and transactional data | Compute and read scaling |
| **Azure Cosmos DB** | NoSQL database | Visitor context and operational data requiring flexible scale | Autoscale throughput |
| **Azure Data Lake Storage** | Data lake | Raw, validated, and curated analytical data | Managed capacity |
| **Azure Machine Learning** | ML platform | Training, registry, deployment, and monitoring | Endpoint and compute scaling |
| **Azure OpenAI** | Generative AI | Grounded visitor assistant capabilities | Managed deployment capacity |
| **Azure AI Search** | Search and retrieval | Approved knowledge retrieval for RAG | Scale replicas and partitions |
| **Azure Key Vault** | Secrets and keys | Secrets, keys, and certificate protection | Managed service |
| **Azure Monitor / Application Insights** | Observability | Metrics, logs, traces, dashboards, and alerts | Managed ingestion and retention |

> **Note:** Final Azure service selection, SKU, region, capacity, and cost must be validated through ADRs, non-functional requirements, load testing, and cost analysis.

---

## Cross-Cutting Concerns

### Security

- **Identity:** Microsoft Entra ID for workforce identities and an approved customer identity approach for visitors.
- **Authorization:** Role-based and attribute-aware access where appropriate.
- **API Protection:** OAuth 2.0 / OpenID Connect, validation policies, throttling, and request controls.
- **Device Security:** Unique device identity, certificate-based authentication, secure provisioning, and rotation.
- **Encryption:** TLS for data in transit and platform-supported encryption for data at rest.
- **Secrets:** Azure Key Vault and managed identities where supported.
- **Network:** Segmentation, private endpoints where justified, firewall controls, and restricted administrative access.
- **Privacy:** Data minimization, consent where required, retention controls, and restricted use of visitor and animal-care data.

**Reference:** [Security Design](../05-Operations/SecurityDesign.md)

---

### Observability

- **Metrics:** Platform, application, integration, device, business, and AI quality metrics.
- **Logs:** Centralized structured logging with appropriate retention and access controls.
- **Traces:** Distributed tracing across APIs, services, events, and AI orchestration.
- **Dashboards:** Visitor operations, animal monitoring, device health, platform health, and AI quality.
- **Alerts:** Service degradation, message backlog, device disconnect, anomalous error rate, AI quality degradation, and cost threshold alerts.
- **Correlation:** Trace and correlation identifiers propagated across end-to-end flows.

**Candidate KPIs and SLOs:**

- Ticket purchase success rate
- API latency and error rate
- Event-processing delay and dead-letter count
- Device connectivity and telemetry freshness
- Alert delivery success
- AI groundedness, model quality, drift, and fallback rate
- Platform availability and recovery objectives

> Final numeric targets must be agreed and documented in the non-functional requirements and validated through testing.

**Reference:** [Observability and Operations](../05-Operations/Observability.md)

---

### Resilience

- **Retries:** Bounded retries with exponential backoff and jitter.
- **Circuit Breakers:** Prevent repeated calls to unhealthy dependencies.
- **Bulkheads:** Isolate critical ticketing and animal-monitoring workloads.
- **Timeouts:** Define dependency-specific timeout budgets.
- **Dead-Letter Handling:** Preserve failed asynchronous messages for controlled investigation and replay.
- **Graceful Degradation:** Use cached, rule-based, or limited-function responses when dependencies or AI services are unavailable.
- **Offline Operation:** Buffer telemetry on the edge and replay securely after connectivity is restored.
- **Disaster Recovery:** Define backup, restore, regional recovery, RTO, and RPO according to business criticality.

**Example:** If the recommendation model is unavailable, return safe rule-based suggestions rather than blocking core ticketing or visitor information services.

---

### Data Consistency

- **Strong Consistency:** Payment, confirmed ticket, family pass, and entitlement transactions.
- **Eventual Consistency:** Analytics, recommendations, telemetry, notifications, and reporting.
- **Idempotency:** Required for payment requests, ticket creation, event consumers, and telemetry replay.
- **Auditability:** Important business and operational changes must be traceable.
- **Event Sourcing:** Apply only where its audit and reconstruction benefits justify the additional complexity.

**Trade-off:** Strong consistency protects critical transactions but may reduce availability during dependency failures. Eventual consistency improves decoupling and resilience but requires reconciliation and clear user-state handling.

---

## Performance and Reliability Targets

This HLD records candidate measures. Final targets must be agreed with the owners of validation and operations.

The following measures must be finalized during non-functional requirement definition. Unvalidated numeric targets should not be presented as commitments.

| Metric | Proposed Target | Validation Method |
|---|---:|---|
| API latency | TBD | Load and performance testing |
| Ticket purchase completion time | TBD | End-to-end testing |
| Telemetry processing delay | TBD | Stream monitoring and load testing |
| Device connectivity | TBD | Device health monitoring |
| AI inference latency | TBD by use case | Model endpoint monitoring |
| AI groundedness and accuracy | TBD by use case | Evaluation dataset and human review |
| Availability | TBD by business capability | SLO and uptime monitoring |
| Recovery time objective | TBD by service criticality | Disaster-recovery exercise |
| Recovery point objective | TBD by data class | Backup and restore validation |

---

## Cost Considerations


Cost must be estimated after the workload assumptions, Azure regions, retention periods, service tiers, AI usage, and traffic volumes are agreed.

| Scenario | Primary Cost Drivers | Optimization Considerations |
|---|---|---|
| **Ticket Purchase** | API, application compute, database, payment integration, messaging | Autoscaling, right-sizing, caching, and transaction monitoring |
| **Visitor Insights** | Device events, stream processing, storage, analytics | Sampling where appropriate, storage lifecycle, and batch aggregation |
| **Animal Monitoring** | Sensors, cameras, edge compute, IoT ingestion, AI inference | Edge filtering, event prioritization, model selection, and retention policy |
| **Recommendations** | Feature retrieval, model inference, cache, data processing | Cache eligible results, select suitable model size, and monitor inference volume |
| **AI Visitor Assistant** | Search retrieval, token usage, model deployment, content processing | Grounding scope, token controls, caching, routing, and provider abstraction |
| **Observability** | Log ingestion, metric retention, traces, dashboards | Sampling, retention tiers, filtering, and archive policies |



---

## Architecture Trade-Offs

The detailed analysis is recorded in [Architecture Trade-Off Analysis](tradeoffs/architecture_tradeoffs.md).

### Edge vs Cloud

- **Edge advantages:** Offline support, local buffering, reduced latency, and reduced unnecessary data transfer.
- **Edge limitations:** Distributed operations, device lifecycle complexity, constrained compute, and additional security responsibilities.
- **Cloud advantages:** Centralized governance, elastic processing, managed analytics, and access to advanced AI services.
- **Cloud limitations:** Network dependency, data transfer considerations, and potential latency.
- **Proposed direction:** Hybrid edge and cloud, with essential local processing at the estate and centralized analytics and AI in Azure.

### Event-Driven vs Request-Response

- **Event-driven advantages:** Loose coupling, resilience, scalable consumers, and support for telemetry and analytics.
- **Event-driven limitations:** Eventual consistency, duplicate delivery handling, tracing complexity, and operational overhead.
- **Request-response advantages:** Immediate feedback and simpler user-facing transaction flow.
- **Request-response limitations:** Runtime dependency coupling and cascading-failure risk.
- **Proposed direction:** Request-response for immediate visitor transactions; asynchronous messaging for telemetry, notifications, analytics, and downstream workflows.

### Monolith vs Modular Services

- **Monolith advantages:** Simpler initial deployment and reduced distributed-system overhead.
- **Monolith limitations:** Coupled releases and coarse scaling as capabilities grow.
- **Modular-service advantages:** Clear capability ownership, independent scaling, and technology isolation where justified.
- **Modular-service limitations:** Greater deployment, monitoring, testing, and data-consistency complexity.
- **Proposed direction:** Begin with clearly separated business modules and extract independently deployed services only when scale, ownership, risk, or release cadence justifies the change.

---

## Assumptions and Constraints

- Wi-Fi coverage across the estate is patchy.
- MQTT-capable hardware can be installed throughout the estate.
- Cloud services may be used, but estate-to-cloud connectivity must be addressed.
- AI-assisted outputs affecting animal welfare require appropriate human review.
- Final technology SKUs, regions, numeric SLOs, and cost estimates require validation.
- External integrations, data availability, retention rules, and identity journeys must be confirmed during detailed design.


