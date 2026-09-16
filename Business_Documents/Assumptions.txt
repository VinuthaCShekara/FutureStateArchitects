# Assumptions

## 1. Purpose

This document records the assumptions used to develop the Von Digitalis Estates target architecture. Each assumption must be validated by an accountable stakeholder. If an assumption proves incorrect, the affected design, ADR, cost estimate, delivery plan, or risk assessment must be reviewed.

## 2. Business Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-BUS-01 | The initial solution scope includes visitor services, estate operations, IoT telemetry, analytics, and approved AI-assisted use cases. | Business Owner | To Be Validated |
| A-BUS-02 | Existing ticketing, payment, and operational systems can expose approved integration interfaces or data exchanges. | Business and Integration Owners | To Be Validated |
| A-BUS-03 | Business owners will define critical journeys, operating hours, service priorities, and acceptable degraded modes. | Business Owner | To Be Validated |
| A-BUS-04 | Qualified personnel remain accountable for animal-care, safety, security, and other high-impact decisions. | Domain Owners | To Be Validated |
| A-BUS-05 | AI capabilities are advisory or assistive unless a separate approval authorizes greater automation. | Business and AI Governance Owners | To Be Validated |

## 3. User and Access Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-USR-01 | Visitor, employee, operator, animal-care, support, and administrative roles can be clearly defined. | Business and Security Owners | To Be Validated |
| A-USR-02 | Administrative and employee access will use approved enterprise identity controls. | Security Owner | To Be Validated |
| A-USR-03 | Visitor authentication requirements will vary by business journey and data sensitivity. | Product and Security Owners | To Be Validated |
| A-USR-04 | Accessibility, browser, device, language, and usability requirements will be supplied before final acceptance testing. | Product Owner | To Be Validated |

## 4. Cloud and Platform Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-PLT-01 | Microsoft Azure is the preferred target cloud, subject to the approved cloud-platform ADR. | Architecture Owner | To Be Validated |
| A-PLT-02 | Required subscriptions, landing zones, identity integration, networking, policies, and deployment permissions will be available. | Cloud Platform Owner | To Be Validated |
| A-PLT-03 | Managed services are preferred when they satisfy security, availability, performance, integration, and cost requirements. | Architecture Owner | To Be Validated |
| A-PLT-04 | Separate environments will be available for development, integration, testing, pre-production, and production as approved. | DevOps and Platform Owners | To Be Validated |
| A-PLT-05 | Infrastructure and environment configuration will be managed through version-controlled Infrastructure as Code. | DevOps Owner | To Be Validated |

## 5. Connectivity and Edge Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-EDG-01 | Estate locations have network connectivity, but temporary interruption and variable quality are expected. | Estate Technology Owner | To Be Validated |
| A-EDG-02 | Approved edge gateways can buffer critical events and synchronize after connectivity is restored. | IoT and Edge Owner | To Be Validated |
| A-EDG-03 | Devices and gateways can support approved identity, certificate, encryption, time synchronization, and update controls. | IoT Security Owner | To Be Validated |
| A-EDG-04 | Offline business capabilities and maximum offline duration will be defined by stakeholders. | Business and Operations Owners | To Be Validated |
| A-EDG-05 | MQTT topics and payloads can follow documented naming, schema, versioning, and access-control standards. | IoT Architecture Owner | To Be Validated |

## 6. Data Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-DAT-01 | Authoritative sources and ownership can be identified for visitor, ticketing, attraction, animal, device, and operational data. | Data Owner | To Be Validated |
| A-DAT-02 | Data classification, residency, retention, deletion, consent, and audit requirements will be provided before production. | Privacy and Compliance Owners | To Be Validated |
| A-DAT-03 | Non-production environments will use masked, anonymized, or synthetic personal data unless an approved exception exists. | Data and Privacy Owners | To Be Validated |
| A-DAT-04 | Common identifiers can be established for estates, attractions, enclosures, animals, devices, and events. | Data Architecture Owner | To Be Validated |
| A-DAT-05 | Data-quality rules will be agreed for completeness, validity, timeliness, duplication, and reconciliation. | Data and Domain Owners | To Be Validated |

## 7. AI Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-AI-01 | Every AI use case will have a defined business objective, owner, approved inputs, output consumer, quality metric, and fallback. | AI Product Owner | To Be Validated |
| A-AI-02 | Representative, legally usable, and expert-reviewed evaluation data can be made available. | Data and Domain Owners | To Be Validated |
| A-AI-03 | AI-generated animal-care insights will be reviewed by qualified personnel and will not be treated as autonomous diagnosis or treatment. | Animal-Care Owner | To Be Validated |
| A-AI-04 | Approved models and AI services can meet the required security, privacy, safety, latency, and cost controls. | AI Architecture and Governance Owners | To Be Validated |
| A-AI-05 | Models, prompts, configurations, grounding sources, and evaluation results can be versioned and audited. | AI Engineering Owner | To Be Validated |
| A-AI-06 | Low-confidence, unsafe, unavailable, or ungrounded AI responses will use an approved fallback or human-review path. | AI Product and Operations Owners | To Be Validated |

## 8. Security and Compliance Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-SEC-01 | Least privilege, role-based access, managed identities, and centralized secret management are mandatory platform controls. | Security Owner | To Be Validated |
| A-SEC-02 | Encryption is required in transit and at rest using organization-approved standards. | Security Owner | To Be Validated |
| A-SEC-03 | Security events, administrative actions, deployments, access changes, and high-impact AI decisions require auditable records. | Security and Compliance Owners | To Be Validated |
| A-SEC-04 | Public APIs and device endpoints will be protected through authentication, authorization, validation, throttling, and threat controls. | API and Security Owners | To Be Validated |
| A-SEC-05 | Compliance obligations and evidence-retention requirements will be confirmed before production approval. | Compliance Owner | To Be Validated |

## 9. Operations and Delivery Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-OPS-01 | Operational ownership, support coverage, escalation paths, and severity definitions will be documented. | Operations Owner | To Be Validated |
| A-OPS-02 | Critical services will expose health checks, logs, metrics, traces, dashboards, alerts, and correlation identifiers. | Observability Owner | To Be Validated |
| A-OPS-03 | CI/CD pipelines will enforce automated build, test, security, policy, artifact, deployment, and rollback controls. | DevOps Owner | To Be Validated |
| A-OPS-04 | Availability, performance, capacity, RTO, RPO, retention, and rollback targets will be approved before production. | Business and Operations Owners | To Be Validated |
| A-OPS-05 | Backup restoration, service recovery, edge resynchronization, and rollback will be tested periodically. | Operations and Platform Owners | To Be Validated |

## 10. Cost Assumptions

| ID | Assumption | Validation Owner | Status |
|---|---|---|---|
| A-CST-01 | Budgets and cost thresholds will be defined for environments and major workloads. | Product and FinOps Owners | To Be Validated |
| A-CST-02 | Resource tagging or equivalent allocation mechanisms will support cost visibility. | Platform and FinOps Owners | To Be Validated |
| A-CST-03 | AI, storage, image/video, telemetry, and non-production consumption will have usage guardrails. | Product, AI, and FinOps Owners | To Be Validated |
| A-CST-04 | Cost optimization will not override mandatory security, privacy, safety, resilience, or compliance controls. | Architecture and Governance Owners | To Be Validated |

## 11. Assumption Review Process

1. Assign an accountable validation owner to every assumption.
2. Record supporting evidence or a decision reference.
3. Update the status to Validated, Rejected, Superseded, or Open.
4. If rejected, identify affected ADRs, diagrams, NFRs, risks, estimates, and delivery plans.
5. Review open assumptions at architecture checkpoints and before production approval.
