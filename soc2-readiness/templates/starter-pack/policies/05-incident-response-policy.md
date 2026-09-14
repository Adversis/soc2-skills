# Incident Response Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME employees and contractors

---

## 1. Purpose

This policy establishes ACME's commitment to detecting, responding to, and learning from security incidents. ACME maintains logging of access and changes to production systems (retained for a minimum of 90 days) to support detection and investigation, and configures [alerts for security-relevant events (such as identity or access policy changes, production configuration changes, and authentication anomalies)] so that suspected incidents can be triaged promptly. The Incident Response Process describes the specific steps to follow when an incident occurs.

## 2. What Is a Security Incident

A security incident is any event that threatens the confidentiality, integrity, or availability of ACME systems or customer data. Examples include:

- Unauthorized access to systems or data
- Credential compromise (phishing, credential stuffing)
- Data exposure (accidental or intentional)
- Ransomware or malware infection
- Denial of service affecting production systems
- Unauthorized changes to production infrastructure

Suspected incidents should be treated as incidents until confirmed otherwise.

## 3. Reporting

### Internal reporting

All employees are required to report suspected security incidents immediately to the {{SECURITY_OWNER}} or via [{{SECURITY_CONTACT}}](mailto:{{SECURITY_CONTACT}}). Incidents are also tracked in the team's primary chat channel.

There is no penalty for reporting an incident in good faith. Early reporting reduces impact.

### External reporting

ACME maintains a public channel where external parties may report security concerns discovered through legitimate use of the product or its public-facing services. Reports may be submitted by email to [{{SECURITY_CONTACT}}](mailto:{{SECURITY_CONTACT}}); the channel is published on the public security page. ACME does not authorize security testing of its systems and does not operate a bug bounty program; the public page exists so that issues discovered through legitimate means can be received and acted on. 

ACME does not commit to an external acknowledgement or response SLA; expectations are set on the public reporting page, and low-quality, automated, or AI-generated reports may be closed without reply. External reports that warrant action are triaged by the {{SECURITY_OWNER}} under §4 using the same severity model and response steps as internal reports.

## 4. Response Ownership

The {{SECURITY_OWNER}} is the Incident Commander for all security incidents unless they delegate that role. The Incident Commander is responsible for:

- Declaring and scoping the incident
- Coordinating the response
- Communicating with affected stakeholders
- Deciding when the incident is resolved
- Initiating a post-mortem when required

## 5. Incident Log

Sev 1 and Sev 2 incidents are recorded in the team's incident log, including:

- Date detected and date resolved
- Severity (see procedures/ir-incident-response-procedures.md)
- Brief description and scope
- Root cause (where determined)
- Remediation actions taken
- Link to incident record and post-mortem

The Incident Log is reviewed during the annual risk assessment.

## 6. Post-Mortems

Incidents rated Severity 1 or Severity 2 require a written post-mortem within five business days of resolution using the template. Post-mortems focus on systemic improvements, not individual blame. The completed document is stored in the team's documentation system.

## 7. Plan Testing

The {{SECURITY_OWNER}} runs an incident response tabletop at least annually, recording the date, participants, scenario, and findings.

## 8. External Notification

Customer and regulatory notification obligations are governed by the Incident Disclosure Policy.
