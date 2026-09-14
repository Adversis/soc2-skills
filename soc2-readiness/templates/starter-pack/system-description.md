ACME AI, Inc. - SOC 2 Type 1 System Description

[TEMPLATE NOTE — remove before issuing. Every affirmative statement below (single-tenant isolation, encryption in transit/at rest, IAM/config/auth alerting, logged troubleshooting access, retention, etc.) is management's assertion in the SOC report, and the auditor will test what management describes. Before finalizing, walk each claim with the {{SECURITY_OWNER}} and mark it verified / change / delete, with evidence beside every material claim. See the soc2-audit-prep skill's system-description verification walk.]

Company Overview
ACME AI, Inc. (“ACME”) was founded in 2026 and is headquartered in [City, State]. ACME provides an AI-powered customer-support assistant for enterprise support teams. The platform connects (read-only) to a customer's helpdesk and knowledge sources, uses a hosted large language model to draft and summarize support replies, and gives managers dashboards on response quality and trends.
ACME was founded by [Name] (CEO & {{SECURITY_OWNER}}), formerly VP at [prior company], and [Name] (COO & CPO), formerly cybersecurity and audit leader at [prior company].
Services Provided	
ACME is delivered as a hosted web application or as a self-deployed application in a customer’s cloud environment. It ingests support conversations and knowledge content from connected sources, uses a hosted LLM to draft and summarize replies in real time, surfaces analytics to managers, and records user and system actions in an activity log.
ACME provides four primary capabilities:
Connectors: integrate with a customer's helpdesk, knowledge base, and related SaaS sources in read-only mode to ingest support content
Assist: uses a hosted LLM to draft and summarize support replies against the customer's guidelines in real time
Insights: dashboards and trend analysis over the ingested and analyzed conversations
Activity Log: records user and system actions for accountability and investigation
Each customer is provisioned with a dedicated single-tenant deployment within ACME’s {{CLOUD_PROVIDER}} environment to ensure logical isolation.
Principal Service Commitments and System Requirements
ACME’s security commitments are documented in customer agreements (Master Subscription Agreements and Data Processing Addenda) and include:
Logical access controls permit users to access only information and capabilities required for their role.
Single-tenant architecture provides logical isolation between customer environments.
Customer data is encrypted in transit and at rest.
Source code changes are peer-reviewed and pass automated security checks before reaching production.
Personnel are vetted, trained, and granted production access only after identity verification, reference checks, and security onboarding are complete.
Components of the System
Infrastructure
Primary infrastructure used to provide ACME’s services includes the following:
Platform
Type
Purpose
{{CLOUD_PROVIDER}}
Managed Kubernetes
Container orchestration for backend services and ingestion/processing workers.
{{CLOUD_PROVIDER}}
VPC, IAM, logging, KMS
Network isolation, access management, audit logging, and key management.
{{CLOUD_PROVIDER}}
Object storage
Storage for application artifacts, generated outputs, and operational data.
{{VCS}}
Source control and CI/CD ({{VCS}} Actions)
Application and infrastructure code; CI/CD workflows that build, test, and deploy to {{CLOUD_PROVIDER}}.
Terraform
Infrastructure-as-code
Defines and manages {{CLOUD_PROVIDER}} infrastructure; state and modules stored in version control.
{{DATA_STORE}}
Managed database
Operational data store for application state, configuration, and tenant metadata.
{{DATA_STORE}}
Managed analytics database
Analytics and observability; receives content and usage telemetry from processing workers.
{{MODEL_PROVIDER}} API
Model API
LLM inference invoked by the application to draft and summarize support replies.

Runtime Architecture
The platform runtime is centered on {{CLOUD_PROVIDER}}. Backend services and ingestion/processing workers are containerized and deployed in the {{CLOUD_PROVIDER}} environment.
Layer
Hosted In
Responsibility
Backend services
{{CLOUD_PROVIDER}}
Process application requests and write operational records to {{DATA_STORE}}.
Processing workers
{{CLOUD_PROVIDER}}
Call {{MODEL_PROVIDER}} to draft and summarize replies from ingested content.
Ingestion workers
{{CLOUD_PROVIDER}}
Ingest support content, collect usage telemetry, forward records to {{DATA_STORE}}.
Infrastructure code
{{VCS}} + Terraform
Define and manage {{CLOUD_PROVIDER}} resources and runtime infrastructure.

Software
Primary software used to provide ACME’s services includes the following:
Software
Details
Purpose
Primary language runtime
Golang, Python, Rust, Typescript
Primary development language and runtime for ACME applications.
Framework
GRPC
Web application framework powering the platform
{{DATA_STORE}}
Hosted
Operational database (managed via {{DATA_STORE}}).
{{DATA_STORE}}
Hosted
Analytics and observability database (managed via {{DATA_STORE}}).

People
ACME currently has a staff of {{TEAM_SIZE}} organized into the following functional areas:
Management: The CEO and COO are responsible for security, compliance, and business operations. The COO is the security program owner and incident commander.
Product Development: Engineers design and maintain the platform, peer-review all code and infrastructure changes, and architect and deploy the underlying cloud infrastructure.
Product Operations: Engineers serve in an on-call rotation, respond to system alerts, triage customer-reported issues, and initiate incident response for potential security events.
Commercial: Commercial functions are currently performed by the CEO and COO.
Data
ACME handles several categories of data:
Configuration Data: Customer accounts, integration credentials, policy definitions, and audit logs. Stored in {{DATA_STORE}}; access is scoped to each customer’s tenant. ACME personnel may access configuration data to troubleshoot issues with {{SECURITY_OWNER}} approval; such access is logged.
Customer Content Data: support conversations and knowledge content ingested, LLM drafts and summaries, and associated usage metadata. Collected by ingestion workers in {{CLOUD_PROVIDER}} and stored in {{DATA_STORE}} per customer-configured retention policies. May include sensitive customer information depending on the content the customer submits. Telemetry routed to {{MODEL_PROVIDER}} for inference is subject to {{MODEL_PROVIDER}}’s data handling and retention terms.
Log Data: Operational logs produced by ACME services, written to {{DATA_STORE}} and {{CLOUD_PROVIDER}} logging. May include snapshots of Configuration Data and metadata about telemetry processing. Retained for a limited period and automatically deleted.
All data is encrypted in transit (TLS 1.2+) and at rest (AES-256) across ACME’s managed databases and cloud storage.
Processes and Procedures
Formal IT policies and procedures exist that describe physical security, logical access, computer operations, change control, and data communication standards. All personnel are expected to adhere to the ACME policies and procedures that define how services are delivered. These are located in the company’s documentation system and can be accessed by any ACME team member.
Physical Security
Production data is hosted by {{CLOUD_PROVIDER}}, {{DATA_STORE}}, and {{DATA_STORE}}; ACME personnel have no physical access to those data centers. 
ACME maintains office space with physical access controls, including badged entry to the building and office suite and keyed interior doors; access is limited to authorized personnel and managed through {{OFFICE_RECEPTION}}. Employees may also work remotely. 
Company-issued devices are configured with full-disk encryption, automatic screen lock, automatic OS updates, and remote-wipe capability.
Logical Access
ACME enforces least-privilege access via role-based controls (RBAC). Personnel access corporate systems using their {{IDP}} account, which serves as the federated identity source for SaaS systems that support “Sign in with {{IDP}}.” 
For systems that do not support federated sign-in, personnel use standalone accounts with MFA and strong, unique passwords stored in an approved password manager. ACME does not operate enterprise SSO with SCIM provisioning; access is tracked through an Access Control Matrix and Onboarding/Offboarding Checklists.
End-user authentication for the ACME application uses {{APP_AUTH}} for enterprise SSO and {{APP_AUTH}} as the application authentication layer.
MFA is required for source control, the {{CLOUD_PROVIDER}} console, identity provider administrative consoles, and any system that contains customer data.
Onboarding includes provisioning accounts, identity and work-authorization verification, reference checks, policy acknowledgment, and security training. When personnel depart, accounts are revoked no later than one business day after the last day of work.
Computer Operations - Backups
Customer data is automatically backed up by {{DATA_STORE}} and {{DATA_STORE}} according to their standard backup schedules. 
ACME verifies backup is enabled during the annual vendor review and performs a test restore from each critical backup source at least annually.
Computer Operations - Availability
Any ACME employee can initiate incident response by notifying the {{SECURITY_OWNER}} via {{SECURITY_CONTACT}}, the {{CHAT_CHANNEL}} {{CHAT_PLATFORM}} channel, or direct contact. 
Sev 1–2 incidents target triage within one hour during business hours.
External parties can report suspected security issues to {{SECURITY_CONTACT}}; ACME’s disclosure expectations are published on the public security page. ACME does not operate a bug bounty program.
Dependency and container scanning run in CI, and remediation follows a risk-based timeline per the Patch Management Policy.
Change Control
All application and infrastructure changes are submitted via {{VCS}} pull request, require review and approval before merge, and must pass automated CI checks (tests, dependency scanning, build validation). 
The main branch is protected, and direct pushes are prohibited. Infrastructure changes use Terraform and are reviewed through the same pull-request process; manual console changes are documented in the Change/Release Log within one business day.
Data Communications
Production infrastructure runs on {{CLOUD_PROVIDER}}-managed services. All external traffic is HTTPS; internal service communication uses TLS 1.2+. 
ACME does not maintain a corporate network or VPN; personnel access services over encrypted connections on the public internet.
Boundaries of the System
This report covers ACME services hosted on {{CLOUD_PROVIDER}}, including the web application, customer-dedicated single-tenant deployments, and the corporate systems that support the hosted services.
Not in scope:
On-premise ACME deployments operated by customers
Customer-managed infrastructure, data sources, and helpdesk/knowledge systems connected via Connectors
Subservice organization internal operations
Customer-side application code and end-user systems
The Applicable Trust Services Criteria and the Related Controls
Common Criteria (Security Category)
Security refers to the protection of (i) information during its collection or creation, use, processing, transmission, and storage, and (ii) systems that use electronic information to process, transmit, or transfer, and store information to enable the entity to meet its objectives. Controls over security prevent or detect the breakdown or circumvention of segregation of duties, system failures, incorrect processing, theft or other unauthorized removal of information or system resources, misuse of software, and improper access to, use of, alteration, destruction, or disclosure of information.
Availability, Confidentiality, Processing Integrity, and Privacy are not in scope for this report.
Control Environment
Integrity and Ethical Values
Personnel sign an acknowledgment of ACME’s policies and a confidentiality agreement at hire.
ACME’s code of conduct and acceptable use policy communicate behavioral standards; acknowledgement is required annually.
Identity and work-authorization verification and reference checks are performed for personnel per applicable employment requirements.

Commitment to Competence
Roles have defined competency requirements.
Security training is provided at hire and annually thereafter.

Management’s Philosophy and Operating Style
ACME management balances rapid development in a fast-evolving AI infrastructure market with responsible stewardship of sensitive customer data. Management reviews material changes to the business to ensure compatibility with customer obligations and is briefed periodically on regulatory and industry changes affecting the services.

Organizational Structure and Assignment of Authority and Responsibility
ACME operates a flat structure; all personnel report to the CEO or COO. The COO owns the security program. An organizational chart documents areas of authority and is communicated to personnel.
Human Resource Policies and Practices
New personnel acknowledge ACME’s policies and sign a confidentiality agreement at onboarding.
An annual security review includes acknowledgment of the Acceptable Use Policy and Code of Conduct.
Personnel termination is executed by the COO per the Offboarding Checklist.
Risk Assessment Process
ACME maintains a risk register that tracks risks to the platform's confidentiality, integrity, and availability. Risks are evaluated based on likelihood and impact; high-scoring risks are assigned remediation tasks and tracked throughout the product development process. The register is reviewed annually; risks identified outside the annual cycle are addressed as they arise. The assessment includes the potential for fraud and management override of controls.
Integration with Risk Assessment
The environment in which the system operates, the commitments, agreements, and responsibilities of ACME’s system, and the nature of the system's components result in risks that the criteria will not be met. ACME addresses these risks by implementing suitably designed controls that provide reasonable assurance that the criteria are met. Because each system and the environment in which it operates are unique, the combination of risks that meet the criteria and the controls necessary to address them will be unique. As part of the system's design and operation, ACME’s management identifies the specific risks that the criteria will not be met and the controls necessary to address them.
Information and Communications Systems
Internal communication uses {{CHAT_PLATFORM}} ({{CHAT_CHANNEL}} for operational and security coordination) and email. Engineering work is tracked in {{VCS}}. 
Compliance evidence, training records, the vendor register, and the asset inventory are maintained in {{GRC_PLATFORM}}. 
Customer communications use email and, for enterprise customers, dedicated channels established during onboarding.
Monitoring Controls
Automated alerts are configured for IAM and access-policy changes, production configuration changes, and authentication anomalies. 
The {{SECURITY_OWNER}} or delegate reviews security-relevant events and evaluates any suspected control failures. 
Deficiencies are recorded in the risk register and addressed based on severity.
Changes to the System
No significant changes to the system are described as of the report date.
Incidents
No incidents requiring disclosure as of the report date.
Criteria Not Applicable to the System
The Trust Services Criteria for Availability, Confidentiality, Processing Integrity, and Privacy are not applicable to this examination, as only the Security category is in scope.
Subservice Organizations
ACME’s services are designed with the assumption that certain controls will be implemented by subservice organizations. Such controls are called complementary subservice organization controls. It is not feasible for all of the trust services criteria related to ACME’s services to be solely achieved by ACME’s controls. Accordingly, subservice organizations, in conjunction with the services, should establish their own internal controls or procedures to complement those of ACME.
ACME uses the carve-out method for the following subservice organizations:
Subservice Organization
Service Provided
{{CLOUD_PROVIDER}}
Cloud infrastructure (compute, storage, networking, IAM, KMS, logging)
{{DATA_STORE}}
Managed operational database
{{DATA_STORE}}
Managed analytics and observability database
{{MODEL_PROVIDER}}
LLM inference API
{{APP_AUTH}}
Identity provider for application user SSO
{{APP_AUTH}}
Application authentication layer
{{VCS}}
Source code management and CI/CD
{{IDP}}
Corporate email, documents, calendar

The following subservice organization controls should be implemented by these providers to provide additional assurance that the trust services criteria described within this report are met:
Subservice Organization Controls - Common Criteria / Security
Criteria
Control
Applicable Provider(s)
CC6.4
Physical access to data centers is approved by an authorized individual.
{{CLOUD_PROVIDER}}, {{DATA_STORE}}, {{DATA_STORE}}
CC6.4
Physical access points to server locations are recorded by closed-circuit television.
{{CLOUD_PROVIDER}}, {{DATA_STORE}}, {{DATA_STORE}}
CC6.4
Physical access points to server locations are managed by electronic access control devices.
{{CLOUD_PROVIDER}}, {{DATA_STORE}}, {{DATA_STORE}}
CC6.4
Electronic intrusion detection systems are installed within server locations to monitor and alert on security incidents.
{{CLOUD_PROVIDER}}, {{DATA_STORE}}, {{DATA_STORE}}
CC6.1
Logical access to the underlying infrastructure is restricted to authorized personnel and reviewed periodically.
All
CC6.7
Encryption is enforced for data at rest and in transit at the platform level.
{{CLOUD_PROVIDER}}, {{DATA_STORE}}, {{DATA_STORE}}, {{MODEL_PROVIDER}}
CC7.1
Vulnerabilities in the underlying platform are identified and remediated.
All
CC6.1
Authentication services for application users maintain availability and integrity.
{{APP_AUTH}}, {{APP_AUTH}}, {{IDP}}
CC8.1
Source control and CI infrastructure protect code integrity and access.
{{VCS}}

ACME monitors subservice organization controls by reviewing SOC 2 or ISO 27001 attestation reports or other risk management procedures annually, and monitoring external communications, including incidents and customer complaints relevant to services provided by the subservice organization.
Complementary User Entity Controls
ACME’s services are designed with the assumption that certain controls will be implemented by user entities. The following complementary user entity controls should be implemented by user entities:
User entities are responsible for understanding and complying with their contractual obligations to ACME.
User entities are responsible for notifying ACME of changes to technical or administrative contact information.
User entities are responsible for managing user accounts within their ACME tenant (provisioning, deprovisioning, and role assignment) and for configuring SSO and enforcing multi-factor authentication on their identity provider.
User entities are responsible for protecting API keys, service account credentials, and other secrets used to access ACME.
User entities are responsible for the security and compliance of the data sources, helpdesk, and knowledge systems they connect to ACME, and for reviewing the content they route through ACME against their own policies.
User entities are responsible for understanding that prompts and telemetry routed to {{MODEL_PROVIDER}} are subject to {{MODEL_PROVIDER}}’s standard data handling and retention policies and for ensuring that content submitted to ACME complies with {{MODEL_PROVIDER}}’s terms.
User entities are responsible for monitoring ACME audit logs and alerts for activity within their tenant.
User entities are responsible for promptly notifying ACME of any actual or suspected information security incidents involving their tenant, including compromised user accounts.
