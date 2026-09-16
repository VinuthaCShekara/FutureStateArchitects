# Problem Statement

## 1. Business Context

Von Digitalis Estates operates visitor attractions and estate facilities that require coordinated visitor services, operational monitoring, animal-care support, and data-driven decision-making. The proposed solution introduces an integrated digital platform supported by cloud, edge, IoT, analytics, and AI capabilities.

## 2. Current Challenges

The current operating model presents the following challenges:

- Visitor journeys such as ticket purchasing, pass validation, attraction discovery, and navigation are not delivered through one consistent digital experience.
- Estate teams have limited real-time visibility into visitor demand, attraction popularity, crowding, and operational conditions.
- Animal-health, feeding, environmental, and population information may originate from multiple devices and systems, making timely analysis difficult.
- Manual processes and disconnected systems can delay operational decisions and increase support effort.
- Intermittent estate connectivity can interrupt device communication and access to cloud-hosted services.
- Data from visitor, operational, animal-care, and IoT sources requires secure integration and appropriate governance.
- AI-enabled recommendations require validation, explainability, safety controls, monitoring, and human oversight.

## 3. Problem Statement

Von Digitalis Estates needs a secure, resilient, scalable, and observable digital platform that unifies visitor services, estate operations, IoT telemetry, analytics, and AI-assisted capabilities. The platform must improve the visitor experience and operational decision-making while continuing to support critical estate activities during temporary connectivity or downstream-service failures.

The solution must avoid creating unacceptable risks relating to privacy, security, animal welfare, AI accuracy, service reliability, vendor dependency, or operating cost.

## 4. Business Impact

If these challenges are not addressed, the organization may experience:

- Inconsistent visitor experiences.
- Delayed response to crowding or operational issues.
- Reduced visibility into animal-care and environmental indicators.
- Higher manual effort and support costs.
- Difficulty scaling services during peak periods or across additional estates.
- Increased security, privacy, and compliance risk.
- Loss of trust caused by unreliable or unsafe AI recommendations.

## 5. Required Business Capabilities

The target solution should enable:

1. Online ticket and family-pass purchasing.
2. Secure and reliable pass validation.
3. Visitor navigation and attraction information.
4. Personalized recommendations with safe fallback behaviour.
5. Attraction popularity and crowding insights.
6. Animal-health, feeding, environmental, and population monitoring.
7. Secure IoT and MQTT-based telemetry ingestion.
8. Edge buffering and synchronization during connectivity interruptions.
9. Operational dashboards, alerts, logs, metrics, and traces.
10. Governed AI evaluation, deployment, monitoring, and human review.

## 6. Desired Outcome

The desired outcome is an integrated estate platform that:

- Improves visitor convenience and engagement.
- Provides timely and reliable operational insight.
- Supports qualified personnel with AI-assisted recommendations rather than autonomous high-impact decisions.
- Protects visitor, operational, and animal-care data.
- Supports repeatable deployment, testing, monitoring, recovery, and controlled change.
- Can evolve as business demand, estate locations, connected devices, and AI use cases grow.

## 7. Scope Boundary

### In Scope

- Visitor-facing digital services.
- Ticketing and pass-validation integration.
- Visitor guidance and recommendation capabilities.
- IoT and edge-based telemetry collection.
- Operational and animal-care analytics.
- AI-assisted visitor and operational use cases.
- Security, privacy, observability, deployment, testing, and resilience controls.

### Out of Scope Unless Later Approved

- Replacement of every existing estate system.
- Autonomous animal diagnosis or treatment.
- Autonomous safety, security, or financial decisions.
- Detailed payment-provider implementation.
- Final production SLA, RTO, RPO, data-retention, and AI-quality targets before stakeholder approval.
