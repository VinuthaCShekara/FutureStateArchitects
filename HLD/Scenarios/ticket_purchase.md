## Ticket Purchase and Family Pass

### Business Context

The estate needs a dependable digital journey for individual tickets and family passes. Payment and ticket entitlement must remain accurate even if downstream notification or analytics processing is delayed.

### Actors
- Visitor
- Ticketing Service
- Family Pass Service
- Payment Provider
- Notification Service
- Support Staff

### Key Components
- Web or mobile visitor channel
- Azure Front Door
- Azure API Management
- Ticketing and Family Pass services
- Azure SQL Database
- Payment integration
- Azure Service Bus
- Notification consumer

### Main Flow

![Ticket Purchase and Family Pass Sequence Diagram](Ticket_Purchase_and_Family_Pass_Sequence_Diagram.png)

### Data Flow
- The visitor submits product, attendee, and payment-related information through the protected channel.
- The ticketing service validates availability, price, and request idempotency.
- The payment provider returns an authorization outcome.
- Confirmed order and entitlement records are committed transactionally.
- A ticket-issued event triggers notification and analytics processing.

### Error Handling and Fallback
- Reject invalid or unauthorized requests with a clear visitor message.
- Use an idempotency key to prevent duplicate orders on retry.
- Do not issue a confirmed ticket when payment authorization fails.
- Retry transient messaging failures; route exhausted messages to a dead-letter queue.
- Allow notification failure without reversing a valid confirmed ticket.

### Security and Privacy Considerations
- Protect APIs using approved visitor identity and access controls.
- Avoid storing raw payment credentials in the estate platform.
- Encrypt sensitive data in transit and at rest.
- Restrict order and entitlement access by role and purpose.
- Audit material order, refund, and entitlement changes.

### Performance and Scalability Considerations
- Scale visitor-facing APIs horizontally for peak arrival periods.
- Keep payment and ticket confirmation on the synchronous critical path.
- Move notifications and analytics to asynchronous consumers.
- Define response-time and availability targets during NFR validation.

### Observability
- Purchase success and failure rate
- Payment integration errors
- API latency and timeout rate
- Duplicate-request detection
- Queue backlog and dead-letter count
- Notification delivery outcome

### Assumptions and Open Decisions
- The payment provider and refund rules require confirmation.
- Ticket pricing and family-pass business rules require confirmation.
- Numeric latency, availability, RTO, and RPO targets are TBD.

### Related Architecture
- [High-Level Design](../README.md)
- [End-to-End Architecture](../diagrams/end_to_end_architecture.md)
- [Data Flow](../diagrams/data_flow.md)
