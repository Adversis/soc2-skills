# Incident Response Process

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME employees and contractors

---

## 1. Purpose

This document describes the step-by-step process ACME follows when responding to a security incident. It supplements the Incident Response Policy.

## 2. Severity Levels

| Severity | Description | Examples |
|----------|-------------|---------|
| **Sev 1 — Critical** | Active breach or imminent threat to customer data or production availability | Active attacker in production, confirmed data exfiltration, ransomware |
| **Sev 2 — High** | Significant security event with potential customer impact | Compromised employee credential, exposed API key with production access, production outage with security dimension |
| **Sev 3 — Medium** | Security event with limited scope and no confirmed data impact | Failed intrusion attempt, suspicious activity under investigation, non-production system compromise |
| **Sev 4 — Low** | Minor policy violation or security hygiene issue | Password reuse discovered, device without MDM enrollment, misconfigured non-sensitive resource |

Sev 1 and Sev 2 are logged per Incident Response Policy §5. Sev 3 and Sev 4 are handled informally without a formal log entry.

## 3. Response Steps

### Step 1: Detect and Report

Any employee who identifies a suspected incident reports it immediately to the {{SECURITY_OWNER}} and posts in the {{CHAT_CHANNEL}} {{CHAT_PLATFORM}} channel. Include:

- What you observed
- When you first noticed it
- Which systems or data may be affected

### Step 2: Triage and Severity Assignment

The Incident Commander ({{SECURITY_OWNER}}, or delegate) assesses the report as soon as practicable — target one business hour for Sev 1–2 — and:

- Assigns a severity level
- Opens an **incident record** in the team's documentation system tagged as a security incident
- Confirms or expands the incident scope

### Step 3: Containment

For Sev 1–2, immediate containment steps are taken before full investigation:

- Revoke compromised credentials
- Isolate affected systems or services
- Disable affected API keys or OAuth tokens
- Block attacking IP addresses if applicable

Document all containment actions in the incident record.

### Step 4: Investigation

The Incident Commander coordinates investigation to determine:

- Root cause of the incident
- Full scope of affected systems and data
- Timeline of attacker or error activity
- Whether customer data was accessed or exfiltrated

### Step 5: Remediation

Remediation actions are tracked in the incident record. All changes to production infrastructure follow the Change Management Policy, with the exception that Sev 1 incidents may require immediate action before a PR review — document the emergency change and create a retroactive PR promptly for peer review.

### Step 6: Resolution

The Incident Commander declares the incident resolved when:

- The root cause is addressed
- Containment and remediation actions are complete
- Monitoring is in place to detect recurrence

Mark the incident record closed and add a resolution summary. Sev 1 and Sev 2 incidents are recorded in the team's incident log per Incident Response Policy §5.

### Step 7: Post-Mortem (Sev 1 and Sev 2)

Within five business days of resolution:

- Write a post-mortem using the IR Post-Mortem Template and store it in the team's documentation system
- Focus on: what happened, why, what was done, what will prevent recurrence
- Review action items with the team

## 4. Customer and Regulatory Notification

If investigation confirms that customer data was involved, follow the Incident Disclosure Policy for notification timing, content, and regulatory obligations.

## 5. Communication

| Audience | When | Channel |
|----------|------|---------|
| Internal team | Immediately on Sev 1–2 detection | {{CHAT_CHANNEL}} {{CHAT_PLATFORM}} |
| {{SECURITY_OWNER}} | Immediately on any incident | Direct message + {{CHAT_CHANNEL}} |
| Customers / regulators | Per Incident Disclosure Policy | Email / legal process |

Do not speculate publicly about incidents before the Incident Commander authorizes external communication.
