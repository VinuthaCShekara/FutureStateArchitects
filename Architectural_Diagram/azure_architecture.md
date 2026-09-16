# Azure Architecture

## Purpose

This document presents the high-level Azure architecture for the **Von Digitalis Estates digital platform**. The design supports digital ticketing, visitor insights, animal monitoring, personalized recommendations, an AI visitor assistant, and resilient IoT processing across areas with intermittent connectivity.

The presentation-ready image is the primary architecture view. The tabular view below explains the layers and major service responsibilities without using a Mermaid diagram.

---

## Presentation-Ready Layered Architecture

![Von Digitalis Estates layered Azure architecture](images/azure_layered_architecture.jpg)

**Figure 1:** Layered Azure architecture showing users, access, applications, edge and IoT, integration, data, AI and analytics, external services, and cross-cutting controls.

---

## Tabular Architecture Diagram

| Layer | Main Components | Primary Responsibility | Key Interactions |
|---|---|---|---|
| **Users and Stakeholders** | Visitors, estate operations staff, animal-care staff, administrators, estate leadership | Use visitor services, review estate operations, validate animal alerts, administer the platform, and review business insights | Access visitor applications and staff dashboards |
| **Access Layer** | Azure Front Door, Web Application Firewall, Azure API Management | Protect public endpoints, route traffic, enforce API policies, throttle requests, and manage API versions | Receives HTTPS traffic and forwards approved requests to application services |
| **Application Services** | Ticketing Service, Visitor Service, Animal Monitoring Service, Recommendation Service, AI Assistant Service, Notification Service | Deliver the main business capabilities of the estate platform | Use APIs for immediate requests and events for downstream processing |
| **Edge and IoT Layer** | MQTT sensors, approved cameras, scanners, counters, Edge Gateway, Azure IoT Hub | Collect estate observations, validate messages, buffer data locally, and securely ingest telemetry into Azure | Sends device telemetry through the Edge Gateway to Azure IoT Hub |
| **Integration and Event Layer** | Azure Service Bus, Azure Event Grid, Azure Functions | Decouple producers and consumers, route events, process messages, retry failures, and support dead-letter handling | Connects application and IoT events to data, notifications, and workflows |
| **Data Layer** | Azure SQL Database, Azure Cosmos DB, Azure Data Lake Storage | Store transactional, operational, raw, validated, curated, and historical data | Supplies business services, reporting, and approved AI workloads |
| **AI and Analytics Layer** | Azure Machine Learning, Azure AI Search, Azure OpenAI, Power BI | Support prediction, anomaly detection, recommendations, grounded assistance, and dashboards | Uses governed data and returns insights to services and authorized users |
| **External Services** | Payment provider, email, SMS, or push provider | Process payments and deliver communications | Integrate through secure APIs and asynchronous notification flows |
| **Security, Governance, and Operations** | Microsoft Entra ID, Azure Key Vault, Defender for Cloud, Azure Monitor, Application Insights, policies and audit | Provide identity, secrets, security posture, monitoring, governance, compliance, and audit across all layers | Applies as a cross-cutting capability throughout the architecture |

---

## Simplified End-to-End View

| Step | Flow | Description |
|---:|---|---|
| 1 | Users → Access Layer | Visitors and staff use web, mobile, or dashboard channels through protected public endpoints. |
| 2 | Access Layer → Application Services | Azure Front Door, WAF, and API Management validate and route requests to the appropriate service. |
| 3 | Estate Devices → Edge Gateway → IoT Hub | MQTT devices send telemetry to the Edge Gateway, which buffers messages during connectivity loss and replays them securely. |
| 4 | Applications and IoT Hub → Integration Layer | Business events and device telemetry are distributed through Service Bus, Event Grid, and Functions. |
| 5 | Integration Layer → Data Layer | Validated information is stored in transactional, operational, and analytical data stores. |
| 6 | Data Layer → AI and Analytics | Approved data supports machine learning, search and retrieval, generative AI, and reporting. |
| 7 | AI and Analytics → Applications and Users | Predictions, recommendations, grounded answers, alerts, and dashboards are returned to the appropriate service or authorized user. |
| 8 | Cross-Cutting Controls → All Layers | Identity, secrets, monitoring, security posture, governance, and audit apply across the complete solution. |

---

## Architecture Layer Details

### 1. Users and Stakeholders

- **Visitors:** Purchase tickets and family passes, receive recommendations, and use the AI visitor assistant.
- **Estate Operations Staff:** Review attraction status, visitor distribution, crowd insights, and operational alerts.
- **Animal Care Staff:** Review animal health, feeding, and population observations.
- **Platform Administrators:** Configure, secure, monitor, and support the platform.
- **Estate Leadership:** Review business, visitor, animal-welfare, and operational reports.

### 2. Access Layer

- **Azure Front Door and WAF:** Provide public routing and web protection.
- **Azure API Management:** Applies access policies, validation, throttling, routing, versioning, and API-level observability.
- Visitor and staff channels do not directly call internal services.

### 3. Application Services

- **Ticketing Service:** Tickets, family passes, bookings, confirmations, and entitlements.
- **Visitor Service:** Profiles, preferences, consent, visit history, and personalization context.
- **Animal Monitoring Service:** Health observations, feeding and population monitoring, alerts, and human review.
- **Recommendation Service:** Personalized and rule-based attraction suggestions.
- **AI Assistant Service:** Grounded estate information and itinerary support.
- **Notification Service:** Ticket confirmations and approved email, SMS, push, and operational notifications.

Core ticketing remains independent of AI availability.

### 4. Edge and IoT Layer

MQTT-capable sensors, cameras, scanners, and counters connect through an Edge Gateway. The gateway performs validation, local buffering, connectivity monitoring, and store-and-forward replay. Azure IoT Hub provides secure device connectivity and cloud ingestion.

Cloud consumers handle duplicates and late-arriving events because buffered telemetry may be replayed after connectivity is restored.

### 5. Integration and Event Layer

- **Azure Service Bus:** Reliable queues, topics, retries, and dead-letter handling for business messages.
- **Azure Event Grid:** Event distribution and publish-subscribe integration.
- **Azure Functions:** Event processing, validation, transformation, notifications, and lightweight workflows.

Immediate visitor transactions use request-response APIs. Telemetry, notifications, analytics, and downstream workflows use asynchronous processing where appropriate.

### 6. Data Layer

- **Azure SQL Database:** Ticket, order, payment-status, pass, booking, and entitlement records.
- **Azure Cosmos DB:** Flexible visitor context, attraction status, current observations, and operational data.
- **Azure Data Lake Storage:** Raw, validated, curated, historical, analytical, training, and evaluation data.

The data lifecycle separates raw, validated, and curated data and applies appropriate quality, lineage, privacy, retention, and access controls.

### 7. AI and Analytics Layer

- **Azure Machine Learning:** Model training, registration, deployment, inference, quality monitoring, and drift monitoring.
- **Azure AI Search:** Retrieval from approved estate information.
- **Azure OpenAI:** Grounded conversational visitor assistance.
- **Power BI:** Visitor, ticketing, animal-monitoring, device-health, operations, AI-quality, and leadership dashboards.

AI outputs affecting animal welfare require confidence checks and authorized human review. The visitor assistant generates responses from approved retrieved content and provides a safe fallback when information is unavailable.

### 8. External Services

- **Payment Provider:** Authorizes payments. The platform avoids storing raw payment credentials and uses idempotency and reconciliation controls.
- **Notification Provider:** Delivers email, SMS, or push messages. Notification failure does not reverse a valid ticket transaction.

### 9. Cross-Cutting Security, Governance, and Operations

- **Microsoft Entra ID:** Workforce identity and access management.
- **Azure Key Vault:** Secrets, keys, credentials, and certificates.
- **Defender for Cloud:** Security posture and supported workload-protection insights.
- **Azure Monitor and Application Insights:** Metrics, logs, traces, dashboards, alerts, and end-to-end correlation.
- **Governance and Audit:** Policies, access reviews, data classification, retention, model approval, cost monitoring, and compliance evidence.

---

## Primary Business Flows

### Ticket Purchase

| Stage | Components |
|---|---|
| Visitor access | Web or mobile application → Azure Front Door and WAF |
| API processing | Azure API Management → Ticketing Service |
| Payment | Ticketing Service → External Payment Provider |
| Transaction | Ticketing Service → Azure SQL Database |
| Downstream processing | Service Bus event → Notification Service and analytics consumers |

### IoT Telemetry

| Stage | Components |
|---|---|
| Data capture | Sensors, cameras, scanners, or counters |
| Estate processing | MQTT → Edge Gateway → validation and buffering |
| Cloud ingestion | Edge Gateway → Azure IoT Hub |
| Event processing | IoT Hub → Event Grid, Service Bus, or Functions |
| Storage and insight | Operational store and Data Lake → analytics or AI |

### Animal Monitoring

| Stage | Components |
|---|---|
| Observation | Animal or enclosure device |
| Processing | Edge Gateway → IoT Hub → data-quality validation |
| AI analysis | Machine-learning or vision model |
| Control | Confidence and policy check |
| Decision | Authorized human review → verified alert or care action |

### AI Visitor Assistant

| Stage | Components |
|---|---|
| Question | Visitor → API Management → AI Assistant Service |
| Retrieval | AI Assistant Service → Azure AI Search → approved estate knowledge |
| Generation | Retrieved context → Azure OpenAI |
| Validation | Safety and groundedness checks |
| Response | Grounded response, limitation, fallback, or escalation |

---

## Resilience and Failure Handling

- Bounded retries with exponential backoff
- Circuit breakers for unstable dependencies
- Timeout policies for synchronous calls
- Idempotency for ticketing, payment, events, and telemetry replay
- Dead-letter queues for unprocessable messages
- Edge store-and-forward during connectivity interruptions
- Duplicate and late-event handling
- Graceful degradation when AI is unavailable
- Rule-based or cached recommendation fallback
- Basic search or FAQ fallback for generative AI
- Failure isolation between ticketing and AI services

---

## Key Architecture Decisions

| Decision Area | Selected Direction | Reason |
|---|---|---|
| Edge vs Cloud | Hybrid edge and Azure cloud | Supports patchy connectivity while providing centralized analytics, AI, governance, and monitoring |
| Request-Response vs Event-Driven | Use both by interaction | Immediate feedback for visitor transactions and decoupling for telemetry, notifications, and analytics |
| Monolith vs Modular Services | Modular architecture with selective independent deployment | Preserves clear boundaries without introducing unnecessary distributed complexity |
| AI Usage | Grounded and human-governed AI | Reduces unsupported responses and keeps animal-care decisions under authorized human control |

---

## Assumptions and Open Decisions

- MQTT-capable devices can be installed across the estate.
- Estate Wi-Fi and external connectivity may be intermittent.
- Edge Gateway hardware and local storage sizing require validation.
- Device count, event frequency, and message size require confirmation.
- Azure regions, service tiers, and capacities require confirmation.
- Performance, availability, RTO, and RPO targets require agreement.
- Visitor identity and consent journeys require confirmation.
- Payment and notification providers require final selection.
- Data retention and privacy requirements require approval.
- AI models, confidence thresholds, and evaluation targets require validation.
- Animal-care experts define approved observations, labels, thresholds, and review workflows.


