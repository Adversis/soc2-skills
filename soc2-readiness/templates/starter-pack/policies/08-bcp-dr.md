# Business Continuity and Disaster Recovery Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME production systems and data

---

## 1. Purpose

This policy establishes ACME's approach to maintaining service availability and recovering from disruptive events that affect production systems or data.

## 2. Recovery Objectives

| Objective | Target |
|-----------|--------|
| Recovery Time Objective (RTO) | [24 hours]{.cmt note="this is a commitment that may be tested for operating effectiveness in a future Type II audit. Update this especially if contracts demand a tighter SLA."} |
| Recovery Point Objective (RPO) | 24 hours |

These targets reflect ACME's current scale and reliance on managed cloud services. They will be revisited as enterprise customer commitments require tighter SLAs.

## 3. Infrastructure and Redundancy

ACME's production infrastructure is hosted on a major cloud platform. Workloads and data stores run on managed cloud services whose availability, replication, and failover are governed by the respective provider's SLA.

ACME relies on its cloud providers' availability commitments. The {{SECURITY_OWNER}} reviews provider SLAs and incident histories annually as part of the vendor review.

## 4. Backup and Recovery

Backup of critical data stores is performed by the underlying managed services. Each provider runs automated backups at least daily per its standard configuration; ACME's role is to confirm backup is enabled at appropriate retention — verified during the annual vendor review — and to test recoverability. Infrastructure configuration is managed as code in version control and is recoverable from any commit. The specific systems, providers, retention periods, and restore procedures are documented in the Backup Recovery Runbook.

The {{SECURITY_OWNER}} [validates backup recoverability at least **once per year** by performing a test restore from each critical backup source]{.cmt note="at least one documented restore test should exist before audit. Log tests in the Backup Recovery Runbook or appropriate location."}. Results are documented in the Backup Recovery Runbook.

## 5. Disaster Recovery Response

A disruptive failure affecting production systems or customer data is handled as a security incident under the Incident Response Policy. Restoration follows the procedures documented in the Backup Recovery Runbook, against the RTO and RPO targets in §2. Customer communication for material outages is governed by the Incident Disclosure Policy. Any outage exceeding two hours requires a written post-mortem.

## 6. Annual Walkthrough

The {{SECURITY_OWNER}} conducts an annual BCP/DR walkthrough to:

- Review and update the Backup Recovery Runbook
- Confirm recovery contacts and escalation paths are current
- Verify that backup configurations remain correct
- Document the walkthrough outcome

The walkthrough may be a tabletop exercise (discussion-based) rather than a live recovery test, provided that individual backup restoration tests have been conducted separately.

## 7. Dependencies

ACME's recovery capability depends on the availability of the cloud platform, managed data services, source control, and the corporate access and communication tools the team relies on. Specific dependency details are documented in the Backup Recovery Runbook.

The RTO/RPO targets in §2 apply to failures ACME can remediate — data corruption or loss, accidental deletion, a failed deploy, or a bounded managed-service failure recoverable from backup or by redeploying infrastructure-as-code. A total, prolonged loss of a primary cloud region is a provider-level event whose restoration timing depends on the provider; ACME's obligation there is to follow the provider's recovery and to keep customers informed under the Incident Disclosure Policy.
