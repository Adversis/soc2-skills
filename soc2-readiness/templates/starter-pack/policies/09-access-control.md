# Access Control Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME systems, data, and personnel

---

## 1. Purpose

This policy governs how access to ACME systems and data is granted, managed, and revoked.

## 2. Principles

**Least privilege:** Employees and systems are granted only the access necessary for their role. Default access is no access; access is explicitly granted. Access to Restricted data, including customer AI conversation data, is limited to personnel with a documented business need.

**Individual accounts:** Shared accounts are prohibited. Every person and automated system uses its own credential.

**Identity management:** Each employee has an individual account on each corporate system. Where a system supports sign-in with the corporate identity provider account (or a comparable federated authentication option), employees must use that mechanism rather than create a standalone account — this anchors authentication to a single corporate identity with MFA enforced at the identity provider level. Federated sign-in is treated as a *partial* deprovisioning control: disabling the identity provider account blocks new logins but does not automatically remove vendor-side user records or revoke active sessions, so the Offboarding Checklist enumerates every vendor regardless of authentication method. For systems that do not support federated sign-in, employees use a standalone account that meets the requirements of the Password Policy.

ACME does not currently operate enterprise SSO with SCIM provisioning across its corporate IT stack. The ACME product uses centralized SSO for application users — that is governed by product architecture, not this policy. Service accounts and API keys are documented in the Access Control Matrix (see §7).

## 3. Access Provisioning

New access requests are initiated by the employee or their manager and approved by the {{SECURITY_OWNER}}. Access is provisioned following the Onboarding Checklist for new hires, or as a discrete request for additional access.

The Access Control Matrix is updated whenever access is granted.

## 4. Privileged Access

Privileged access (administrative roles on production systems and infrastructure) is limited to the minimum number of people required to operate the business. The {{SECURITY_OWNER}} reviews who holds privileged access at least annually.

Production access is not used for development or testing. Where possible, production access requires a separate credential or justification step.

## 5. Access Review

The {{SECURITY_OWNER}} reviews the Access Control Matrix at least **annually** to confirm that all access remains appropriate. Access that is no longer needed is revoked.

The review is also triggered by a change in an employee's role.

## 6. Offboarding

When an employee or contractor departs, [the Offboarding Checklist must be completed as soon as practicable, and no later than **one business day** after their last day]{.cmt note="Review the Offboarding Checklist for accuracy (will cover IDP, source control, cloud, CI/CD, direct service accounts). For Type I no completed evidence is required if there have been no departures; for Type II you'll want timestamped evidence."}. This includes:

- Disabling their account on every corporate system in the Access Control Matrix (cloud platform, source control, data platforms, communication tools, identity-provider admin consoles, CI/CD, and any other system they were granted access to)
- Revoking any API keys, SSH keys, or service-account credentials they had access to or knowledge of
- Recovering or wiping company-issued devices per the Physical Security Policy

The {{SECURITY_OWNER}} confirms completion and updates the Access Control Matrix.

## 7. Service Accounts and API Keys

API keys and service account credentials must:

- Be stored in a secrets management system, not in code or environment files committed to source control
- Be scoped to the minimum permissions required
- Be documented in the Access Control Matrix with owner and expiry (if applicable)
- Be rotated when an employee with knowledge of the credential departs

CI/CD pipeline secrets (used exclusively by automated pipelines) may be stored in the CI/CD platform's native secrets management.

## 8. Remote Access

ACME has no on-premises infrastructure. Remote access to cloud infrastructure is via individual user accounts with MFA enforced at the platform level. No VPN is required.

Network-level access controls (cloud platform firewall rules) restrict inbound production traffic at the cloud infrastructure layer.
