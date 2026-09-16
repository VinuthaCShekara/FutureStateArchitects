# Azure Security Document
## 1. Document Purpose

This document defines the security architecture, controls, and operational procedures implemented within the Azure Integration Platform to ensure confidentiality, integrity, and availability of business data and services.

## 2. Scope

The scope includes:
- Azure API Management (APIM)
- Azure Logic Apps
- Azure Functions
- Azure Service Bus
- Azure Key Vault
- Azure Storage Account
- Azure Monitor & Log Analytics
- Application Gateway with WAF
- Microsoft Entra ID (Azure AD)

## 3. Security Objectives
- Confidentiality
- Protect sensitive information from unauthorised access.
- Encrypt data at rest and in transit.
- Integrity
- Ensure data is not altered without authorisation.
- Maintain audit trails for all transactions.
- Availability
- Ensure business- critical services remain operational.
- Implement monitoring, alerting, and disaster recovery mechanisms.

## 4. Identity and Access Management
- Authentication
- Microsoft Entra ID authentication enabled.
- Multi- Factor Authentication (MFA) enforced.
- Conditional Access policies configured.
- Authorization
- Role- Based Access Control (RBAC) implemented.
- Least- privilege principle followed.
- Privileged Access
- Privileged Identity Management (PIM) enabled.
- Administrative access reviewed periodically.
- ## 5. Network Security
- Network Segmentation
- Resources deployed within approved virtual networks.
- Access restricted using Network Security Groups (NSGs).
- Private Connectivity
- Private Endpoints used where supported.
- Public access disabled when not required.
- Perimeter Security
- Application Gateway + WAF

#### Incoming requests are routed through Application Gateway protected by Web Application Firewall (WAF). WAF protects against:

- SQL Injection
- Cross- Site Scripting (XSS)
- OWASP Top 10 vulnerabilities

## 6. Secrets and Key Management
#### Azure Key Vault

### The following secrets are stored in Key Vault:

#### API Keys
- Connection Strings
- Certificates
- OAuth Secrets
- Service Credentials
- Controls
- RBAC enabled
- Soft Delete enabled
- Purge Protection enabled
- Secret rotation policy implemented

Azure Key Vault is identified as the recommended secret- management solution.

## 7. API Security
### Azure API Management

#### All API traffic is secured through:

- OAuth 2.0 authentication
- JWT validation
- Rate limiting
- IP filtering
- Request validation

The internal security guidance specifically references OAuth 2.0 validation and policy- based access controls in APIM.

## 8. Integration Security
## Azure Logic Apps

### Security controls include:

- Managed Identity authentication
- Secure Inputs and Outputs enabled
- Parameterised configuration
- Diagnostic logging enabled

#Azure Functions

## Controls:

- Managed Identity
- Restricted network access
- Application settings secured in Key Vault
- Monitoring enabled
- Service Bus

## Controls:

- RBAC authorisation
- Managed Identity access
- Network restrictions
- Audit logging

## 9. Data Protection
- Encryption at Rest

## Enabled for:

- Storage Accounts
- Databases
- Service Bus
- Encryption in Transit
- TLS 1.2 or higher mandatory
- HTTPS enforced
- Data Retention
- Retention policies implemented according to business requirements.
- Audit logs retained in Log Analytics.

## 10. Monitoring and Incident Management
### Monitoring

### Tools used:

### Azure Monitor
- Log Analytics
- Application Insights
- Microsoft Defender for Cloud
- Alerts

### Security alerts generated for:

- Failed authentication attempts
- Excessive API failures
- Resource availability issues
- Security recommendations

## 11. Vulnerability Management

### Activities include:

- Monthly vulnerability review
- Security patch verification
- Defender recommendations review
- Annual security assessment

## 12. Backup and Disaster Recovery
- Backup Strategy
- Resource configuration backed up.
- Storage redundancy configured.
- Recovery procedures documented.
- Recovery Objectives
- Objective	ValueRPO	Defined by business requirements
- RTO	Defined by business requirements
