# Offboarding Checklist

Directions: complete this checklist and save the completed artifact in the employee's folder.

**Employee:** _Name_  
**Last day:** _YYYY-MM-DD_  
**Completed by:** _Name_  
**Completed on:** _YYYY-MM-DD_

Target: complete all steps within 1 business day of the last day. Document any delays and reasons in Notes.

---

## Immediate (same day as departure)

- [ ] Disable the corporate {{IDP}} account — blocks federated sign-in across vendors that use "Sign in with {{IDP}}"
- [ ] Force-sign-out / terminate active sessions where the option exists ({{IDP}} > Security > sign out from all sessions; vendor-side where available)
- [ ] Change any shared credentials the employee had knowledge of

## Within 1 Business Day — Per-vendor verification

For **every** system listed in the **Auth** tab of the Access Control Matrix, act according to the auth method:

| Auth Method | Action |
|---|---|
| **Federated**, password fallback **disabled** (verified) | Confirm the user can no longer sign in. No vendor-side action required beyond {{IDP}} account disable. |
| **Federated**, password fallback **enabled** | Disable or delete the user account at the vendor. |
| **Standalone** | Disable or delete the user account at the vendor. |
| **Native {{IDP}}** | Already handled by the {{IDP}} account disable above. |
| **API-only** | See "API Keys & Secrets" below. |

Common vendors to check explicitly — confirm each by name, do not skip:

### Source Control

- [ ] {{VCS}} — remove from organization
- [ ] Revoke any personal access tokens, SSH keys, or deploy keys associated with their {{VCS}} account

### Cloud Infrastructure

- [ ] {{CLOUD_PROVIDER}} — revoke project access / remove from IAM groups
- [ ] Remove from any {{CLOUD_PROVIDER}} service-account `actAs` or impersonation bindings
- [ ] Rotate any {{CLOUD_PROVIDER}} service-account keys the employee had access to or could read from the secrets store

### Data Platforms

- [ ] {{DATA_STORE}} — remove direct access if granted _(one line per managed data platform)_

### Application Infrastructure / IdPs

- [ ] {{APP_AUTH}} — remove admin role if held
- [ ] {{APP_AUTH}} console — remove access if held
- [ ] {{MODEL_PROVIDER}} platform — remove access if granted

### Communications & Productivity

- [ ] {{CHAT_PLATFORM}} — remove from workspace (or rely on {{IDP}} account disable if confirmed federated)

### Compliance / Operations

- [ ] {{GRC_PLATFORM}} — remove access

### Anything Else

- [ ] Cross-check every remaining row in the **Auth** tab. Anything not explicitly listed above gets handled per the auth-method table.

## API Keys & Secrets

- [ ] Using the **Service Accounts** tab as the index, inspect each platform's native inventory for credentials the employee created or knew (PATs, OAuth grants, service-account keys, shared secrets)
- [ ] Rotate each one and update all consumers (CI/CD, application config, anywhere the secret is referenced)
- [ ] Confirm the rotated credential is in the secrets manager and the old one is invalidated at the source

## Device

- [ ] Recover the device; log serial and return date in the device disposal/lifecycle log (`procedures/device-disposal-log.csv`)
- [ ] Cryptographic erase performed and verified — device boots to a clean Setup Assistant; record method, verifier, and date in the log. (Full-disk encryption was enabled; no Restricted/customer data is stored locally per Data Retention §7)
- [ ] If MDM-enrolled: also issue remote wipe and confirm; the live device inventory reflects removal
- [ ] Mark the device Returned/Decommissioned in the log. If not returned, handle as lost/stolen per Physical Security Policy §6

## Physical Access

- [ ] Recover physical keys to ACME office and conference room; update the Asset Register
- [ ] Request {{OFFICE_RECEPTION}} deactivate building and floor badge access

## Update Records

- [ ] Update the **People** tab of the Access Control Matrix — set Status to `Departed` and fill in Departed date
- [ ] Update the Asset Register — mark device returned / wiped

## Confirm Complete

- [ ] {{SECURITY_OWNER}} sign-off: all access revoked as of _YYYY-MM-DD_

---

## Notes

_Exceptions, delayed items, device not yet returned, password-fallback findings, etc. Document any deviation from the 1-business-day target and the reason._
