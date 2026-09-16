# Data Flow

## Purpose

This document summarizes the principal data-flow patterns used across the HLD.

---

## Pattern 1: Synchronous Request-Response

Use this pattern when the visitor channel requires an immediate response from a business operation.

![Pattern_Synchronous_Request_Response.png

**Used for:** Ticket queries, purchase initiation, profile lookup, and immediate visitor requests.

**Consistency:** Strong consistency for payment status, confirmed tickets, passes, and entitlements.

---

## Pattern 2: Asynchronous Business Events

Use this pattern to decouple the producing business service from notification, analytics, and operational consumers.

Pattern_2_Asynchronous_Business_Events.png

**Used for:** Ticket lifecycle events, payment outcomes, notifications, and analytics.

**Controls:**
- Idempotency
- Retries
- Schema versioning
- Correlation tracking
- Dead-letter handling

---

## Pattern 3: IoT Telemetry

Use this pattern to collect telemetry from estate sensors, cameras, and scanners through resilient edge buffering.

![Pattern 3 - IoT Telemetry

**Used for:** Environmental readings, feeding observations, population observations, attraction counters, and device health.

**Controls:**
- Device authentication
- Validation
- Store-and-forward buffering
- Duplicate handling
- Late-event processing

---

## Pattern 4: Batch Analytics and Model Training

Use this pattern for governed batch analytics and controlled model training and deployment.

Pattern_4_Batch_Analytics_and_Model_Training.png

**Used for:** Historical trends, reporting, feature preparation, and model training.

**Controls:**
- Data quality
- Lineage
- Privacy
- Repeatability
- Approval workflow
- Versioning

---

## Pattern 5: Grounded Generative AI

Use this pattern to generate responses from approved estate knowledge with safety and groundedness checks.

Pattern_5_Grounded_Generative_AI.png

**Used for:** Estate information, attraction discovery, and itinerary assistance.

**Controls:**
- Approved content
- Prompt protection
- Grounding validation
- Content safety
- Evaluation
- Feedback
- Escalation

---

## Data Classification Guidance

### Transactional
Orders, payment references, tickets, passes, and entitlements.

### Visitor
Profile, preferences, consent, interaction, and feedback data.

### Operational
Attraction status, device health, alerts, and staff actions.

### Animal Care
Sensor observations, feeding records, population observations, review outcomes, and care actions.

### Analytical
Aggregated, validated, and curated datasets.

### AI
Prompts, retrieved sources, responses, model versions, quality results, and feedback, subject to approved retention.
