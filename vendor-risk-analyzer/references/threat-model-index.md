# Threat Model Index

## How to Use This File

For each vendor analysis, scan these breach patterns and assess which ones are relevant given the vendor's described architecture, the integration type, and the data at stake. Not every pattern applies to every vendor — select the 3-7 most realistic scenarios and model them in detail.

**Selection criteria:**
- Does the vendor's architecture create exposure to this pattern?
- Would this pattern result in compromise of the requesting org's data specifically?
- Is this pattern realistic for the vendor's size, industry, and threat profile?

**For each selected pattern, assess:**
- What the SOC 2 report says (or doesn't say) about relevant controls
- Whether the described controls actually mitigate this specific path or just adjacent risks
- The gap between claim and evidence
- Estimated attacker cost (low/medium/high) given the described environment

---

## Breach Patterns

These are ordered roughly by frequency and real-world impact based on incident data, IR firm reporting, and red team experience. The ordering is a starting heuristic, not a ranking — adjust based on the specific vendor context.

---

### 1. Compromised Developer / Engineer Credentials

**How it happens:** Infostealer malware on a developer workstation captures session tokens, SSH keys, API keys, or credentials stored in browser/credential managers. Attacker uses legitimate developer access to reach production systems, source code repositories, CI/CD pipelines, or databases. Alternatively: phished credentials for developer-facing systems (GitHub, GitLab, cloud console, internal tooling).

**Why it's common:** Developers typically have the broadest access — source code, infrastructure, secrets, production databases. A single compromised developer often has enough access to reach the most sensitive data without needing to escalate privileges.

**What to look for in the report:**
- Separation of duties in the SDLC (does a single dev have push-to-prod capability?)
- Code review / merge approval requirements (branch protection)
- Secrets management (are production credentials in the repo, env vars, or a vault?)
- MFA on developer-facing systems (source control, CI/CD, cloud consoles)
- EDR on developer workstations (not just AV)
- Production database access controls (bastion hosts, just-in-time access, query logging)
- Anomaly detection for developer access patterns

**Common SOC 2 gaps:** Reports frequently describe infrastructure access controls (MFA for AWS console) but say nothing about application-layer developer access. SDLC sections describe the workflow but rarely mention security controls within the pipeline. "Anti-malware" on workstations without EDR means infostealers operating in userspace go undetected.

---

### 2. Infrastructure Misconfiguration

**How it happens:** Cloud storage buckets (S3, Azure Blob, GCS) left publicly accessible. Overly permissive security groups or NACLs. Default credentials on internal services. Misconfigured IAM policies granting broader access than intended. Exposed management interfaces (Kubernetes dashboards, database admin panels, CI/CD consoles) on public internet.

**Why it's common:** Cloud environments are complex, permissions are granular, and misconfigurations can happen silently. Infrastructure-as-code helps but doesn't eliminate drift. Most orgs have at least some misconfiguration at any given time.

**What to look for in the report:**
- Cloud security posture management (CSPM) or equivalent automated scanning
- Infrastructure-as-code with drift detection
- Automated policy enforcement (SCPs, Azure Policy, GCP Org Policies)
- Network architecture description — are internal services on private subnets?
- Security group / NACL review processes
- Minimum attack surface language — what's actually exposed to the internet?
- Multi-cloud complexity (more environments = more misconfiguration surface)

**Common SOC 2 gaps:** Reports describe firewalls and security groups exist but not how they're validated. "Monthly vulnerability scanning" may cover hosts but not cloud configuration. Multi-cloud or hybrid setups (e.g., Rackspace + AWS) multiply the misconfiguration surface but rarely get proportional control attention.

---

### 3. Compromised Administrative Credentials

**How it happens:** Phishing, credential stuffing, or session hijack targeting administrative accounts (cloud console admins, domain admins, database administrators). Admin accounts often have the broadest blast radius.

**Why it's common:** Administrative accounts are high-value targets. Even with MFA, session tokens can be stolen, MFA fatigue attacks succeed, and help desk social engineering can reset MFA.

**What to look for in the report:**
- MFA on all administrative access (and what type — push notification MFA is weaker than hardware keys)
- Privileged access management (PAM) — session recording, just-in-time elevation, approval workflows
- Admin account inventory and review cadence
- Break-glass procedures (how are emergency admin actions handled?)
- Conditional access policies (geo-restrictions, device compliance, impossible travel detection)
- Admin access logging and alerting independent of the admin's own systems

**Common SOC 2 gaps:** "MFA required for administrative access" doesn't specify MFA type. Push-notification MFA is vulnerable to fatigue attacks. "Quarterly access reviews" for admin accounts may be too infrequent. Reports rarely describe PAM tooling or session monitoring.

---

### 4. Exposed or Leaked Secrets

**How it happens:** API keys, database credentials, encryption keys, or service account tokens committed to source code repositories (public or private), stored in environment variables that get logged, embedded in container images, or left in configuration files on publicly accessible systems. Also: secrets shared via Slack, email, or ticketing systems.

**Why it's common:** Secrets management is operationally hard. Developers take shortcuts. Even with a vault, secrets leak during debugging, in CI/CD logs, or through third-party integrations.

**What to look for in the report:**
- Secrets management solution (HashiCorp Vault, AWS Secrets Manager, etc.)
- Secret scanning in CI/CD pipelines and repositories
- Credential rotation policies and automation
- Encryption key management (HSM-backed, automated rotation, separation of duties)
- Whether the report mentions secrets management at all (many don't)

**Common SOC 2 gaps:** SOC 2 reports almost never address secrets management specifically. Encryption sections cover data-at-rest and in-transit but skip key management operational details. SDLC sections describe the coding workflow but not the security tooling in the pipeline.

---

### 5. Vulnerable Internet-Facing Application

**How it happens:** Exploitation of web application vulnerabilities — authentication bypass, IDOR, SSRF, injection attacks, API authorization flaws. Attacker goes from unauthenticated internet access to sensitive data through the application itself.

**Why it's common:** Web applications are complex, exposed to the internet by design, and new vulnerabilities are introduced with every deployment. APIs often have weaker access controls than the UI because they're designed for machine-to-machine communication.

**What to look for in the report:**
- Web Application Firewall (WAF) — present but not sufficient alone
- Pen testing scope and frequency (was the API tested? The specific application the customer uses?)
- SDLC security practices (security reviews, SAST/DAST, dependency scanning)
- Authentication architecture (OAuth 2.0, API key management, session management)
- Authorization model (tenant isolation in multi-tenant apps)
- Bug bounty or vulnerability disclosure program
- Input validation and output encoding practices

**Common SOC 2 gaps:** "Annual penetration testing" may not cover the API surface relevant to your integration. WAF is listed as a control but WAFs are trivially bypassable for targeted attacks. Multi-tenant isolation is critical but rarely described in detail.

---

### 6. Supply Chain / Third-Party Compromise

**How it happens:** A vendor's vendor is compromised. Malicious dependency in the software supply chain. Compromised SaaS tool used in the development pipeline. Subservice organization breach that cascades.

**Why it's common:** Modern software depends on hundreds of third-party libraries and services. Vendors depend on subservice organizations for infrastructure, monitoring, authentication, and more. Each one is a potential entry point.

**What to look for in the report:**
- Subservice organization carve-outs — who's carved out and what do they control?
- Vendor/third-party risk management program (is it just annual SOC 2 reviews or is there active monitoring?)
- Software composition analysis (SCA) in the build pipeline
- Dependency management and update practices
- How many subprocessors, and how well governed

**Common SOC 2 gaps:** Subservice organization carve-outs are standard practice but they mean large portions of the control environment are not audited in this report. "Annual review of critical vendors" is compliance hygiene, not supply chain security. Software supply chain security (SBOM, dependency scanning, build provenance) is almost never covered in SOC 2 reports.

---

### 7. Social Engineering / Business Process Exploitation

**How it happens:** Attacker impersonates a customer, executive, or vendor to manipulate internal processes — password resets, access provisioning, wire transfers, data exports. Targets help desk, customer support, finance, or HR.

**Why it's common:** Humans are the most adaptable component in the system, which also makes them the most exploitable. Business processes often have override paths designed for legitimate exceptions that attackers leverage.

**What to look for in the report:**
- Identity verification procedures for sensitive actions (password resets, access changes, data exports)
- Segregation of duties for high-risk operations
- Change management controls for production access modifications
- Whether support/call center operations are in scope (if vendor has one)
- Approval workflows for privileged operations

**Common SOC 2 gaps:** SOC 2 covers access management procedures but rarely describes how identity verification works for inbound requests. Help desk social engineering is one of the most common initial access vectors and is almost never covered in SOC 2 reports.

---

### 8. Insider Threat (Intentional)

**How it happens:** A current employee with legitimate access intentionally exfiltrates data, sabotages systems, or sells access. Motivated by financial gain, grievance, coercion, or ideological reasons.

**Why it's common:** It's less common than external attacks but has outsized impact because the attacker starts inside the trust boundary with legitimate credentials.

**What to look for in the report:**
- Data Loss Prevention (DLP) controls
- Behavioral monitoring / UEBA (User and Entity Behavior Analytics)
- Database access logging and anomaly detection
- Segregation of duties for data access
- Background checks (hiring) — present but limited effectiveness for ongoing risk
- Termination procedures and access revocation timeliness
- Principle of least privilege enforcement and audit

**Common SOC 2 gaps:** SOC 2 covers access controls and termination procedures but rarely addresses detection of legitimate-but-anomalous access patterns. DLP and UEBA are almost never mentioned. "Quarterly access reviews" verify access is appropriate but don't detect misuse of appropriate access.

---

### 9. Ransomware / Destructive Attack

**How it happens:** Initial access via any of the above vectors, followed by lateral movement, privilege escalation, and deployment of ransomware or wipers. Attacker encrypts or destroys data and systems, demands payment.

**Why it's common:** Ransomware is the dominant threat for most organizations. It's industrialized, operates at scale, and exploits the same gaps as other breach patterns.

**What to look for in the report:**
- Backup architecture (immutable backups, offline/air-gapped copies, cross-region)
- Backup testing frequency and documented recovery testing
- Network segmentation (can ransomware spread laterally from initial foothold?)
- EDR coverage and capability (not just AV)
- Disaster recovery plan specifics and test results
- RTO/RPO commitments
- Incident response plan maturity

**Common SOC 2 gaps:** "Daily backups across availability zones" doesn't mean immutable backups. If the backup credentials are accessible from the compromised environment, the backups get encrypted too. DR testing that only covers "different probable scenarios" may not include a full ransomware recovery scenario. RTO/RPO may not be specified.

---

### 10. Data Exposure via Logging / Monitoring / Analytics

**How it happens:** Sensitive data (credentials, PHI, PII, session tokens) inadvertently captured in application logs, error messages, monitoring systems, analytics platforms, or debug outputs. These systems often have broader access controls than the production systems they monitor.

**Why it's common:** Logging everything is standard operational advice. Redacting sensitive data from logs requires deliberate engineering. Log aggregation systems (SIEMs, Splunk, ELK) become secondary datastores of sensitive information with their own access control requirements.

**What to look for in the report:**
- Log content policies (what gets logged, what gets redacted)
- Access controls on logging/monitoring infrastructure
- Data classification enforcement in logging pipelines
- Whether PHI/PII can appear in logs and how it's handled
- Retention policies for logs containing sensitive data

**Common SOC 2 gaps:** SOC 2 reports describe that logging exists and that logs are reviewed but almost never address what data is in the logs or how sensitive data is redacted. SIEM access controls are rarely described separately from general access management.

---

## Assessing Overall Threat Posture

After modeling individual scenarios, assess the overall posture:

**Detection capability:** Can they see an attacker inside their environment? What matters is meaningful detection — a SIEM, cloud-native detection/alerting, or equivalent — and whether they correlate events and can catch lateral movement, not which product they run or whether it's a traditional SIEM. Don't penalize a cloud-native vendor for using cloud-native detection instead of a bolted-on SIEM.

**Response capability:** If they detect something, can they respond fast enough to matter? Incident response plan existence is table stakes — the question is whether the team has practiced, whether they have forensic capability (or retainer), and whether their notification timelines meet your requirements.

**Resilience:** If something goes wrong, how bad does it get? Segmentation, backup architecture, DR testing, and blast radius design determine whether a single compromise cascades into total loss.

**Maturity trajectory:** Is this organization getting better or maintaining the status quo? Evidence of improvement (new controls, framework adoption, dedicated security leadership, increasing test frequency) is a positive signal. Static compliance-minimum posture is a negative one.
