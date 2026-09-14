# ACME AI, Inc. — Security Policy Set
## Meta Document: Overview, Considerations, and Supporting Materials

**Version:** 1.0  
**Date:** {{EFFECTIVE_DATE}}  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME employees, contractors, and systems

---

## Overview

This policy set is adapted from [Tailscale's open-source security policies](https://github.com/tailscale/security-policies) for ACME AI, Inc.'s SOC 2 Type I audit (Security Trust Services Criteria only). It replaces the {{GRC_PLATFORM}}-generated policy package in its entirety.

**Philosophy: Write less, write what you do. Policies name controls; procedures name products.**

The {{GRC_PLATFORM}} templates (~107 pages, 26,600 words) were written for a mid-size company with a dedicated security team, change advisory board, and established ITSM tooling. ACME is a {{TEAM_SIZE}} company. A policy that describes processes you don't actually follow is worse than no policy: it creates audit findings, legal liability, and operational confusion.

This policy set (~40 pages, ~8,000 words) describes what ACME actually does or will do by the audit date. Every commitment in these documents is one an auditor can — and will — verify.

---

## Source and Adaptation

**Source:** Tailscale's security policy framework (MIT License, ~14 policies)  
**Adapted by:** {{ADVISOR}}  
**Scope:** SOC 2 Type I, Security TSC (CC1–CC9)

### Key Adaptations from Tailscale

| Tailscale | ACME |
|-----------|---------|
| Tailscale (company name) | ACME AI, Inc. |
| Security Review Team | {{SECURITY_OWNER}} |
| security@tailscale.com | {{SECURITY_CONTACT}} |
| #security Slack channel | {{CHAT_CHANNEL}} |
| #incidents Slack channel | {{CHAT_CHANNEL}} |
| Internal ticketing system | {{VCS}} Issues |
| Internal tooling references | {{CLOUD_PROVIDER}}, {{VCS}} (named where they are the control) |

### Policies Added (not in Tailscale framework)

- **Physical Security** (15) — SOC 2 CC6.4 requires it; adapted to cloud-native reality (no data center, macOS device management)
- **Encryption** (16) — SOC 2 CC6.1/CC6.7; documents {{CLOUD_PROVIDER}}-managed encryption + key management approach
- **Acceptable Use** (17) — SOC 2 CC1.4/CC6.1; minimal device and data use policy

### Policies Removed (in {{GRC_PLATFORM}}, not here)

- Software Development Lifecycle (rolled into Change Management and Testing)
- Vulnerability Management (rolled into Patch Management)
- Asset Management (rolled into Access Control and Physical Security)
- Logging and Monitoring (covered by CC7.2 in Risk Assessment and Incident Response)
- Third-Party Risk (rolled into Vendor Management)

---

## Open Questions to Confirm with the Client

Confirm each item with the client before the audit; each maps to a control an auditor will test.

1. **Background checks** — Confirm what screening is actually performed (identity/work-authorization and reference checks vs. third-party criminal background checks) and state it as "per applicable employment requirements and company practice." Don't gate system access on E-Verify (it can't be used pre-employment). Retain whatever evidence the real process produces in the personnel file. [Personnel]
2. **Security training** — Confirm the training platform and cadence, and where per-employee completion is tracked. [Personnel, §3]
3. **Retention schedule** — Confirm the customer-data retention period, aligned to the public privacy page ({{PRIVACY_URL}}) and to enterprise contracts. [Data Retention, §4]
4. **Backup testing** — Confirm the cadence and which systems are tested; capture last test date and result in the Backup Recovery Runbook. [BCP/DR, §4]
5. **Offboarding process** — Confirm the Offboarding Checklist enumerates every corporate system (cloud, source control, data platforms, communication tools, identity-provider admin consoles, CI/CD, direct service accounts); without centralized SSO, deprovisioning is manual. For Type I, no completed instance is required if no departures occurred in the relevant period — design assessment only. [Access Control, §3]
6. **Encryption key management** — Confirm whether cloud-managed (provider-default) keys are sufficient or whether any enterprise customer contracts require customer-managed keys. [Encryption, §3]
7. **Vendor register** — Confirm all material vendors are present and that subservice SOC 2 reports are on file. [Vendor Management, §2]
8. **Incident response email** — Confirm {{SECURITY_CONTACT}} exists and is monitored. [Incident Response, §2]

---

## Supporting Documents and Procedures

These are referenced in the policies but are separate operational documents (not policies). Location is noted where confirmed; audit readiness status is indicated.

| Document | Location | Audit Notes |
|----------|----------|-------------|
| Asset Register | {{GRC_PLATFORM}} (agent inventory) | Confirm all devices are enrolled and the list is current |
| Vendor Register | {{GRC_PLATFORM}} (vendor & risk management) | Confirm all material vendors present; SOC 2 reports on file for cloud providers |
| Subprocessor List | {{GRC_PLATFORM}} (vendor management) | Should be exportable; confirm it matches what's disclosed in the privacy policy |
| Risk Register | ⚠️ Not yet created | Needs at least one completed annual assessment on record before audit |
| Access Control Matrix | Spreadsheet | Confirm it reflects current state; auditors will cross-check against provisioning evidence |
| Incident Log | Spreadsheet | Can be empty (no incidents is fine); must exist and be maintained |
| Onboarding Checklist | Shared doc | Confirm it covers all systems; ideally has at least one completed instance with dates |
| Offboarding Checklist | Shared doc | ⚠️ Confirm completeness (see open question #5). For Type I, no completed run is required if no departures occurred as of the as-of date — evidence the zero population instead |
| Backup Recovery Runbook | Shared doc | ⚠️ Needs per-system backup details and last test date populated (see open question #4) |
| Change/Release Log | Shared doc + {{VCS}} PRs | PR history is the primary evidence; shared doc for out-of-band changes |
| Security Training Record | {{GRC_PLATFORM}} (training completion) | Export or screenshot per-employee completion dates before audit |

---

## SOC 2 Trust Services Criteria Coverage

| CC | Criterion | Primary Policy |
|----|-----------|---------------|
| CC1.1–CC1.5 | Control Environment | Personnel, Risk Assessment |
| CC2.1–CC2.3 | Communication | Personnel, Acceptable Use |
| CC3.1–CC3.4 | Risk Assessment | Risk Assessment |
| CC4.1–CC4.2 | Monitoring Activities | Patch Management, Incident Response |
| CC5.1–CC5.3 | Control Activities | Change Management, Access Control |
| CC6.1 | Logical Access | Access Control, Password, Encryption |
| CC6.2 | Access Provisioning | Access Control, Personnel |
| CC6.3 | Access Removal | Access Control, Personnel |
| CC6.4 | Physical Access | Physical Security |
| CC6.5 | Data Disposal | Data Retention, Physical Security |
| CC6.6 | Network Security | Encryption, Access Control |
| CC6.7 | Encryption in Transit | Encryption |
| CC6.8 | Malicious Software | Patch Management, Physical Security |
| CC7.1–CC7.5 | System Operations | Incident Response, Patch Management |
| CC8.1 | Change Management | Change Management, Testing |
| CC9.1–CC9.2 | Risk Mitigation | Vendor Management, BCP/DR |

---

## Audit Preparation Notes

- All policies are effective on adoption date. For Type I, the auditor assesses design as of a point-in-time — policies need to be in place, not necessarily operating for 6+ months.
- Evidence requests will likely include: employee list, vendor list, access provisioning/deprovisioning tickets, training records, incident log (can be empty), change records ({{VCS}} PRs), backup test records.
- The weakest areas for a {{TEAM_SIZE}} company are typically: formal risk register, vendor security assessments, and training records. Plan to have at least one documented instance of each before the audit date.
