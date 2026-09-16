# Observability and Operations

| Document Information | Details |
|---|---|
| **Document ID** | SDD-OBS-001 |
| **Project** | Von Digitalis Estates Digital Platform |
| **Author** | Subhrangada Sarmila Guru |
| **Version** | 1.0 |
| **Date** | September 2026 |
| **Status** | Approved |

## 1. Purpose

This document defines the observability and operational approach for the **Von Digitalis Estates digital platform**. It describes how applications, APIs, integrations, data services, IoT devices, edge gateways, and AI components will be monitored and supported.

The objective is to provide timely visibility into service health, performance, failures, security-relevant events, business outcomes, and operational risks.

## 2. Scope

The observability and operations design covers:

- Visitor web and mobile channels
- Azure Front Door and Azure API Management
- Application services and Azure Functions
- Azure Service Bus and Azure Event Grid
- Azure IoT Hub, field devices, and edge gateways
- Azure SQL Database, Azure Cosmos DB, and Azure Data Lake Storage
- Azure Machine Learning, Azure OpenAI, and Azure AI Search
- Identity, secrets, network, and security controls
- Deployment pipelines and operational processes

## 3. Objectives

The solution will provide:

- Centralized collection of metrics, logs, traces, and audit events
- End-to-end transaction visibility using correlation identifiers
- Early detection of failures, degradation, and capacity risks
- Actionable alerts with clear ownership and response guidance
- Operational dashboards for technical and business stakeholders
- Monitoring for edge connectivity and delayed telemetry
- Monitoring for AI quality, safety, latency, and fallback behavior
- Controlled incident response, escalation, recovery, and improvement

## 4. Observability Principles

1. **Monitor end to end:** Observe the complete user and device journey, not only individual resources.
2. **Use structured telemetry:** Apply consistent fields, timestamps, severity levels, and correlation identifiers.
3. **Alert on impact:** Prioritize alerts that indicate user, animal-care, operational, security, or business impact.
4. **Protect sensitive data:** Do not place credentials, tokens, personal data, or sensitive prompts in logs.
5. **Design for offline operation:** Monitor local buffering, synchronization, replay, and connectivity status.
6. **Automate where practical:** Use automated health checks, notifications, diagnostics, and recovery actions where approved.
7. **Continuously improve:** Update dashboards, alerts, and runbooks using incident and testing outcomes.

## 5. Observability Architecture

```text
Visitor Channels       Business Services       Integration Services
       |                       |                         |
       +-----------------------+-------------------------+
                               |
                    Metrics, Logs and Traces
                               |
             Azure Monitor and Application Insights
                               |
                      Log Analytics Workspace
                               |
              Dashboards, Alerts and Workbooks
                               |
                    Operations and Support Teams

IoT Devices -> Edge Gateway -> IoT Hub -> Processing Services
     |              |             |              |
     +------ Device health, connectivity, telemetry freshness ------+

AI Services -> Model and prompt telemetry -> Quality and safety monitoring
```

## 6. Monitoring by Solution Area

| Solution Area | Monitoring Focus | Example Signals |
|---|---|---|
| Visitor channels | Availability and user experience | Page failures, response time, failed requests, dependency failures |
| Azure Front Door | Entry-point health and protection | Origin health, request volume, latency, blocked requests |
| API Management | API health and policy execution | Request count, latency, error rate, throttling, authentication failures |
| Application services | Application health and performance | Exceptions, dependency latency, resource utilization, failed health checks |
| Azure Functions | Event-processing health | Execution failures, duration, retries, timeout, trigger backlog |
| Service Bus | Messaging reliability | Active messages, dead-letter messages, processing delay, repeated delivery |
| Event Grid | Event delivery | Delivery failures, retry activity, dead-lettered events |
| IoT Hub | Device and telemetry health | Connected devices, disconnects, message volume, rejected messages |
| Edge gateway | Local operational health | Buffer usage, connectivity, synchronization delay, replay failures |
| Data services | Data availability and performance | Connection failures, query performance, capacity, storage growth |
| AI services | Quality, safety, performance, and cost | Latency, groundedness, fallback rate, content-safety events, usage |
| Security services | Access and configuration activity | Failed sign-ins, denied access, secret access, policy changes |

## 7. Metrics

Metrics provide numerical indicators of system health and behavior.

### 7.1 Platform Metrics

- CPU, memory, storage, and network utilization
- Service availability and health-check status
- Request volume, latency, and error rate
- Queue depth, processing delay, and dead-letter count
- Database capacity, connection failures, and query performance
- Device connectivity and telemetry ingestion rate
- Edge buffer utilization and synchronization delay

### 7.2 Business Metrics

- Ticket purchase initiation and completion
- Payment success and failure
- Family pass creation
- Visitor recommendation requests and responses
- Animal-monitoring events and alerts
- Notification delivery success and failure

Business metrics must not expose sensitive visitor, payment, or animal-care information.

### 7.3 AI Metrics

- AI request volume and response latency
- Grounded and ungrounded response indicators
- Content-safety outcomes
- Model or retrieval failures
- Human escalation and fallback usage
- Model version and deployment health
- Token or approved usage indicators

## 8. Logging

All services should generate structured logs using a consistent format.

### 8.1 Required Log Fields

| Field | Purpose |
|---|---|
| Timestamp | Records when the event occurred |
| Severity | Indicates informational, warning, error, or critical status |
| Service name | Identifies the producing service |
| Environment | Identifies development, test, staging, or production |
| Correlation ID | Connects related activities across services |
| Operation name | Identifies the business or technical operation |
| Result | Records success, failure, rejection, or retry |
| Duration | Records operation processing time where applicable |
| Error code | Provides a searchable failure classification |
| Deployment version | Identifies the deployed application or model version |

### 8.2 Logging Controls

Logs must not contain:

- Passwords, secrets, keys, or access tokens
- Full payment information
- Unnecessary personal or visitor information
- Sensitive animal-care notes without approved protection
- Complete AI prompts or responses when they contain sensitive data

Sensitive values must be removed, masked, or tokenized before log ingestion.

## 9. Distributed Tracing

Distributed tracing will connect requests across visitor channels, API Management, application services, messaging components, data services, and AI orchestration.

Each transaction should use a correlation identifier that is:

- Created or accepted at the entry point
- Propagated through synchronous API calls
- Included in asynchronous message metadata
- Recorded in application logs and traces
- Returned in safe error responses where appropriate

Priority journeys for end-to-end tracing include:

- Ticket and family pass purchase
- Payment processing and ticket confirmation
- Visitor recommendation generation
- AI visitor assistant request
- Animal-health alert generation
- IoT telemetry ingestion and replay

## 10. Dashboards

| Dashboard | Intended Audience | Main Content |
|---|---|---|
| Executive service health | Product and architecture stakeholders | Availability, major incidents, critical business journeys |
| Visitor experience | Product and support teams | Ticketing health, API latency, failures, recommendation availability |
| Integration operations | Application and integration support | Queue depth, dead-letter messages, retries, dependency failures |
| IoT and edge operations | Estate and platform operations | Device connectivity, telemetry freshness, buffer status, replay failures |
| Animal monitoring | Authorized animal-care operations | Monitoring pipeline health, delayed events, alert-delivery status |
| AI quality and safety | AI and governance teams | Groundedness, safety outcomes, latency, fallback usage, model health |
| Security monitoring | Security operations | Authentication failures, denied access, suspicious activity, policy changes |
| Cost and capacity | Service owners | Resource consumption, growth patterns, capacity indicators |

Access to dashboards must follow least-privilege principles.

## 11. Alerting Strategy

Alerts must be actionable, prioritized, and assigned to a responsible support group.

| Severity | Meaning | Expected Handling |
|---|---|---|
| Critical | Major service outage, safety-impacting monitoring failure, or severe security event | Immediate investigation and escalation |
| High | Significant degradation or repeated failure affecting an important capability | Prompt investigation and recovery action |
| Medium | Partial degradation, growing backlog, or capacity risk | Review and resolve within the operational process |
| Low | Informational condition or early warning | Review during routine operational monitoring |

### 11.1 Alert Categories

- Service unavailability or failed health checks
- Increased API or application failures
- Sustained latency degradation
- Queue backlog or dead-letter growth
- Device disconnection or stale telemetry
- Edge buffer capacity risk or replay failure
- Database capacity or connection problems
- AI safety, groundedness, latency, or fallback degradation
- Authentication, authorization, or secret-access anomalies
- Backup, deployment, or disaster-recovery failures

### 11.2 Alert Quality Controls

- Avoid duplicate alerts for the same underlying incident
- Use suppression and grouping where appropriate
- Include service, environment, severity, evidence, and runbook link
- Review alert usefulness after incidents
- Remove alerts that do not require action

## 12. Operational Processes

### 12.1 Incident Management

1. Detect and record the incident.
2. Assess impact and assign severity.
3. Identify the affected service and responsible owner.
4. Contain the impact and apply an approved recovery action.
5. Restore service and validate critical journeys.
6. Communicate status through the agreed support channels.
7. Complete a post-incident review for significant incidents.
8. Track corrective actions to completion.

### 12.2 Problem Management

Repeated or high-impact incidents should be investigated to identify root causes. Corrective actions may include code changes, configuration updates, capacity adjustments, improved monitoring, revised runbooks, or architecture changes.

### 12.3 Change and Release Management

- Use controlled deployment pipelines and approval gates
- Link releases to change records where required
- Validate health checks after deployment
- Monitor key indicators during and after release
- Maintain a tested rollback or recovery procedure
- Record application, infrastructure, configuration, and model versions

### 12.4 Service Request Management

Standard operational requests should follow documented procedures for access, configuration, certificates, secrets, dashboard access, data restoration, and device onboarding or removal.

## 13. Runbooks

Operational runbooks should be maintained for:

- API or application outage
- Service Bus backlog and dead-letter processing
- IoT device disconnection
- Edge buffer capacity and telemetry replay
- Database connection or capacity failure
- AI service degradation or unsafe output
- Secret or certificate expiration
- Deployment failure and rollback
- Backup restoration
- Regional service disruption

Each runbook should include prerequisites, diagnostic checks, recovery steps, validation steps, escalation contacts, and evidence requirements.

## 14. Availability, Backup, and Recovery

- Define service-level objectives based on business criticality
- Configure backups according to the approved retention and recovery requirements
- Test restoration procedures regularly
- Document dependencies required for recovery
- Validate RTO and RPO through recovery exercises
- Ensure edge services can buffer essential telemetry during connectivity loss
- Reconcile delayed or replayed messages after service restoration
- Provide graceful degradation when recommendation or AI services are unavailable

Final availability, retention, RTO, and RPO values remain subject to approval in the non-functional requirements.

## 15. Security and Access

- Use Microsoft Entra ID for workforce access
- Apply role-based access control and least privilege
- Use managed identities where supported
- Store secrets and certificates in Azure Key Vault
- Restrict production monitoring and log access
- Audit administrative and configuration changes
- Protect monitoring endpoints and diagnostic settings
- Review privileged access periodically

## 16. Data Retention and Cost Management

- Apply retention according to operational, legal, privacy, and security requirements
- Use appropriate retention tiers for high-volume telemetry
- Filter unnecessary or low-value diagnostic data
- Use sampling only where traceability and incident investigation remain effective
- Archive data only when justified by approved requirements
- Monitor ingestion, storage, query, alerting, and dashboard costs

Retention periods and cost thresholds must be approved before production deployment.

## 17. Ownership and Responsibilities

| Role | Responsibility |
|---|---|
| Service owner | Service health, objectives, risk acceptance, and improvement priorities |
| Application support | Application monitoring, troubleshooting, and incident resolution |
| Integration support | APIs, messaging, retries, and dead-letter handling |
| IoT and edge operations | Device health, connectivity, buffering, synchronization, and replay |
| AI operations | Model deployment, quality, safety, drift, fallback, and usage monitoring |
| Security operations | Security monitoring, investigation, and escalation |
| Platform operations | Azure resource health, capacity, backup, and recovery |
| Architecture team | Design governance, cross-service standards, and major corrective decisions |

Named owners and escalation contacts must be added before production readiness approval.

## 18. Operational Readiness Checklist

- [ ] Monitoring is enabled for all production services
- [ ] Required logs, metrics, traces, and audit events are available
- [ ] Correlation identifiers work across critical journeys
- [ ] Dashboards are created and access-controlled
- [ ] Alerts have tested notification routes and owners
- [ ] Runbooks are complete and accessible
- [ ] Backup and restoration are tested
- [ ] Disaster-recovery procedures are validated
- [ ] Edge buffering and replay are tested
- [ ] AI quality, safety, and fallback monitoring are enabled
- [ ] Sensitive information is excluded or masked in logs
- [ ] Support ownership and escalation paths are documented
- [ ] Post-deployment validation checks are defined
- [ ] Retention and cost controls are approved

## 19. Risks and Mitigations

| Risk | Potential Impact | Mitigation |
|---|---|---|
| Excessive alert volume | Important alerts may be overlooked | Group, prioritize, suppress, and regularly review alerts |
| Missing correlation IDs | Slow root-cause analysis | Enforce correlation standards across APIs, events, and logs |
| Sensitive data in logs | Privacy or security exposure | Apply masking, filtering, access control, and log reviews |
| Edge connectivity loss | Delayed telemetry and reduced visibility | Use local buffering, health monitoring, and controlled replay |
| Monitoring platform failure | Reduced operational awareness | Monitor the monitoring pipeline and retain local diagnostics where required |
| Uncontrolled log growth | Increased cost and slower queries | Apply filtering, retention, archive, and usage monitoring |
| AI quality degradation | Incorrect or unsuitable responses | Monitor quality and safety, use fallback rules, and require human escalation where needed |
| Outdated runbooks | Delayed recovery | Review runbooks after incidents, changes, and recovery exercises |

## 20. Validation Approach

The observability and operations design will be validated through:

- Health-check and synthetic transaction testing
- Log, metric, and trace verification
- Alert notification and escalation testing
- Failure injection and resilience testing
- Queue backlog and dead-letter recovery testing
- Edge disconnection, buffering, and replay testing
- Backup restoration and disaster-recovery exercises
- AI quality, safety, latency, and fallback evaluation
- Operational readiness review before production release

---


