# Password Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME employees and contractors

---

## 1. Purpose

This policy establishes minimum requirements for passwords and authentication for human users. Service accounts, API keys, and other non-interactive credentials are governed by the Access Control Policy §7.

## 2. Authentication

Each employee uses an individual account on each corporate system. Where a system supports sign-in with the corporate identity provider account (or a comparable federated authentication option), employees must use that mechanism rather than create a standalone account. Federated sign-in anchors authentication to a single corporate identity with MFA enforced at the identity provider level, and is treated as a *partial* deprovisioning control — disabling the identity provider account blocks new logins but does not automatically remove vendor-side user records or revoke active sessions. The Offboarding Checklist enumerates each vendor regardless of authentication method.

For systems that do not support federated sign-in, employees use a standalone account that meets the requirements of §3 (MFA) and §4 (Password Requirements).

ACME does not currently operate enterprise SSO with SCIM provisioning across its corporate IT stack. The ACME product uses centralized SSO for application users; that is governed by product architecture, not this policy.

## 3. Multi-Factor Authentication

MFA is required for:

- Source control (enforced by organization policy)
- Cloud platform console access
- Identity-provider administrative consoles (the product IDP today; any future corporate IDP)
- Any other system that contains Confidential or Restricted data

MFA must use an authenticator app (TOTP) or hardware security key. SMS-based MFA is not permitted.

## 4. Password Requirements

Passwords (whether for SSO or for individual system accounts) must:

- Be at least 15 characters
- Not be reused across different systems
- Not be a variant of a previously used password for the same system

Password complexity rules (uppercase, numbers, special characters) are not required, as length is a more effective security control than complexity.

## 5. Password Manager

Employees are expected to use a password manager to generate and store unique passwords. ACME does not mandate a specific product, but recommends one be used for all business accounts.

## 6. Password Rotation

Passwords are not required to rotate on a schedule. Passwords must be changed immediately when:

- The employee has reason to believe it has been compromised
- The {{SECURITY_OWNER}} directs a reset following a security incident
- An employee with shared knowledge of a credential (e.g., a shared service account password) departs

## 7. Credential Sharing

Credentials must not be shared between individuals. See the Access Control Policy for requirements on individual accounts and service account management.

## 8. Default Credentials

Default or vendor-supplied passwords must be changed before any system is connected to the network or used in production.

## 9. Exceptions

Where a system does not support a requirement of this policy — for example, a maximum password length below 15 characters, no MFA option, or no federated sign-in — the {{SECURITY_OWNER}} documents the exception in the Access Control Matrix with: the specific limitation, compensating controls in place (limited scope, reduced retention of sensitive data, shorter rotation cycle, additional monitoring, etc.), and a target date for upgrade or replacement if applicable. Exceptions are reviewed during the annual access review.

This exceptions clause does not extend to service accounts, API keys, or other non-interactive credentials, which are governed by Access Control Policy §7.
