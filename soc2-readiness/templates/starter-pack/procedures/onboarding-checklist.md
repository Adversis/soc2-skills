# Onboarding Checklist

Directions: complete this checklist and save the completed artifact in the employee's folder.

**Employee:** _Name_  
**Start date:** _YYYY-MM-DD_  
**Role:** _Title_  
**Role definition applied:** see Roles tab of the Access Control Matrix, version _YYYY-MM-DD_  
**Completed by:** _Name_  
**Completed on:** _YYYY-MM-DD_

---

## Before Day 1

- [ ] Send welcome email with first-day instructions
- [ ] Order / prepare company device
- [ ] Configure or MDM-enroll the device before handoff (per Physical Security §3)
- [ ] **Assign role** (`CEO-{{SECURITY_OWNER}}`, `COO-CPO`, or `Engineer`) — this determines per-system access per the **Roles** tab of the Access Control Matrix. Note any exceptions to standard role access.

## Day 1 — Access Provisioning

Access is granted per the role definition in the **Roles** tab of the Access Control Matrix. Look up the role and grant the listed level in each system below.

### Identity & SSO

- [ ] Create {{IDP}} account (`firstname.lastname@acme.ai`)
- [ ] Confirm MFA ({{IDP}} 2SV) enrolled before any other access is granted

### Source Control

- [ ] Add to {{VCS}} organization at the level defined for the role
- [ ] Confirm organization-linked and requires MFA

### Cloud Infrastructure

- [ ] Grant {{CLOUD_PROVIDER}} project access at the role-defined level
- [ ] Add to any relevant {{CLOUD_PROVIDER}} groups or service account bindings

### Data / Services

- [ ] {{DATA_STORE}} — grant access at the role-defined level (skip if role = `none`) _(one line per managed data platform)_

### Communication

- [ ] Add to {{CHAT_PLATFORM}} workspace and relevant channels

### Device

- [ ] Hand off company device
- [ ] Confirm device configured or MDM-enrolled per Physical Security §3
- [ ] Confirm full-disk encryption active
- [ ] Confirm screen lock enabled

### Physical Access

- [ ] Request building and floor badge access from {{OFFICE_RECEPTION}}; badge and key records are maintained on file by {{OFFICE_RECEPTION}}
- [ ] Issue physical keys to ACME office and conference room
- [ ] Retain the {{OFFICE_RECEPTION}} badge/key request email and confirmation with this checklist

### Update Records

- [ ] Add a row to the **People** tab of the Access Control Matrix with role, status (Active), joined date, and any exceptions
- [ ] Confirm the device appears in the device inventory ({{GRC_PLATFORM}} agent) with its serial; attach an inventory screenshot to this checklist

## Within First Two Weeks — Security Onboarding

- [ ] Complete security awareness training in {{GRC_PLATFORM}}
- [ ] Acknowledge Acceptable Use Policy in writing (email or {{GRC_PLATFORM}})
- [ ] Walk through: incident reporting process, {{SECURITY_CONTACT}}, {{CHAT_CHANNEL}} {{CHAT_PLATFORM}} channel
- [ ] Walk through: password manager setup, MFA for all accounts

---

## Notes

_Anything unusual about this onboarding — exceptions, delayed access, etc._
