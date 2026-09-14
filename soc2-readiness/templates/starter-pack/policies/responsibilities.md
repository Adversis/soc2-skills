# ACME Security Responsibilities & Tasks

Operational map of the policy set. Three categories:

- **Recurring** — put on the calendar
- **Event-triggered** — run when the trigger fires
- **Continuous controls** — always-on; verified during annual reviews

Owner is COO unless noted.

## Annual tasks

- Access reviews
- Restore test
- IR tabletop
- Vendor reviews
- Security training
- Board meeting
- Policy review

(Onboarding, offboarding, and post-mortems are **event-triggered**, not annual — see that section. What is
annual is the *review* of those procedures, covered by "Policy review.")

## Recurring

### Monthly
- Review open vulnerability alerts; prioritize per Patch Management timelines
- Review documented patch exceptions
- Review enabled security-scanning outputs and open findings (CI dependency/SAST). If external DAST/network scanning is deployed, review its output per its documented cadence

### Quarterly
- Device patch status across the fleet - screenshot "About this Mac"

### Annual (within the audit observation window)
- Risk Register refresh
- Access Control Matrix review (including privileged access)
- Vendor Register review
- Encryption review: at-rest status, key management, no keys in repos
- Backup restoration test from each critical source
- BCP/DR walkthrough; cloud provider SLA + incident history review
- Automated data-deletion verification
- Incident Log review
- EOL tracking refresh for key components
- Public security page review
- Annual security assessment with tracked findings

### Per employee, annual
- Complete security awareness training (tracked in {{GRC_PLATFORM}})
- Acknowledge policy set, AUP, and Code of Conduct (tracked in {{GRC_PLATFORM}})

## Scheduling

Set cadences now so they are in rhythm before the Type 2 observation begins. Schedule the first annual reviews to fall within the Type 2 window — the auditor evidences that annuals happened within the observation period.

### {{GRC_PLATFORM}} (in use)
- Vendor annual review notifications (Vendor Management module)
- Security awareness training reminders (Training module)

Risk Management module is not in use; risk-register cadence is on the calendar (below).

### Calendar (everything else)
Set on the COO calendar or a shared "Security Operations" calendar. Three recurring events cover the operational cadence:

- **Monthly** — "Security ops monthly review" — first Monday, 30 min. Open vulnerability findings, patch exceptions, and any deployed external-scan output.
- **Quarterly** — "Device patch + EDR check" — first Monday of Jan/Apr/Jul/Oct, 30 min. Screenshot "About this Mac" per device.
- **Annual** — "Security annual review week" — pick a week inside the Type 2 observation window. Block ~2h/day across the week; cluster a few items from the Annual list per session.

In each event description, link to this doc and to the specific procedure or artifact you'll touch.

### Setup time
Under an hour total: confirm {{GRC_PLATFORM}} vendor + training workflows have owners assigned (15 min), create the three calendar events (15 min), paste descriptions linking to the relevant procedures (10 min).

## Event-triggered

### Hiring
Before access: identity/work-authorization and reference checks per applicable employment requirements (retain in personnel file). Provision per Onboarding Checklist. Add to Access Control Matrix (ACM) People tab. Complete onboarding training and policy acknowledgments.

### Departure (within 1 business day)
- Complete Offboarding Checklist
- Disable accounts per ACM Auth tab (federated, standalone)
- Revoke PATs, SSH keys, service-account keys the person knew
- Recover or wipe the company device
- Update ACM People tab (Status: Departed; Departed date)
- Rotate shared credentials the departing person knew if any

### Vendor onboarding
Before access to Restricted data or critical infra:
- Security review: SOC 2, questionnaire, risk rationale
- Add to Vendor Register with tier and DPA reference
- If processes customer data: update subprocessor list; check customer notification per contracts

### Vendor termination
- Revoke ACME-granted credentials
- Instruct deletion/return of ACME data per contract
- Mark inactive in Vendor Register with end date

### Production code or infrastructure change
- Via PR; one reviewer other than author; CI passes; no self-approval

### New device issuance
- Enroll in MDM before handover
- Verify: FDE, 15-min auto-lock, remote wipe, auto OS updates, EDR running
- Update Asset Register

### Device disposal
- Full-disk wipe (cryptographic erase acceptable); remove from MDM; update Asset Register
- Certified e-waste or manufacturer recycling (never general trash)

### Sev 1 emergency change
- Document in incident record at deploy
- Retroactive PR promptly for peer review
- {{SECURITY_OWNER}} approves the emergency change record

### Manual cloud-console change (non-code)
- Open `infra-change` Issue using the template
- Get approver confirmation in a comment
- Implement; close with checklist
- Follow up with IaC update where applicable

### New feature with external APIs, customer-data flows, auth logic, or 3rd-party integration
- Security review before production
- Document as PR comment or linked Issue

### Subprocessor list change
- Update subprocessor list
- Notify customers per contracts and Privacy Policy

### Customer relationship ends
- Delete or export data per customer instructions within 30 days
- If no instructions: delete after 30 days; confirm in writing

### Lost or stolen device
- MDM remote wipe on confirmation
- Revoke creds/tokens/keys that may have been on the device
- Log in incident log; confirm with employee what data was accessible

### Credential suspected compromised
- Change immediately; treat as security incident
- {{SECURITY_OWNER}} directs broader reset if wider impact plausible

### CISA KEV / critical CVE hits an ACME-used component
- Confirm presence in stack; start clock from confirmation date
- Apply Patch Management severity * exploit * exposure timeline
- Document exception with {{SECURITY_OWNER}} if timeline cannot be met; review monthly

### Suspected security incident
- Reporter: notify {{SECURITY_OWNER}}; post in team chat
- {{SECURITY_OWNER}}: triage; target 1 business hour for Sev 1–2
- Open incident record in team documentation system
- **Sev 1–2:** immediate containment (revoke creds, isolate, disable tokens). Investigate. Remediate via Change Management (Sev 1 emergency exception allowed). Close with resolution summary. Log in incident log. Post-mortem within 5 business days using IR Post-Mortem Template.
- **Sev 3–4:** handled informally, no formal log.

### Incident involving customer data
- Notify affected customers within 72h of confirming involvement (or contract/regulatory limit)
- Engage counsel for regulatory determination
- Record notifications in incident log

### Customer data deletion request
- Fulfill within 30 days of confirmed written request; confirm in writing
- Legal hold: escalate to {{SECURITY_OWNER}} and counsel

## Where evidence lives

| Document | Location |
|---|---|
| Risk Register | {{GRC_PLATFORM}} |
| Vendor Register | {{GRC_PLATFORM}} |
| Subprocessor list | {{GRC_PLATFORM}} |
| Security Training Records | {{GRC_PLATFORM}} (training completion) |
| Asset Register (devices) | {{GRC_PLATFORM}} (agent inventory) |
| Access Control Matrix | Spreadsheet |
| Incident Log | Spreadsheet |
| Onboarding Checklist | Shared document |
| Offboarding Checklist | Shared document |
| Backup Recovery Runbook | Shared document |
| Change/Release Log | Shared document, {{VCS}} PRs |
| Code & infrastructure changes | {{VCS}} PRs |
| Incident records | Issue tracker, document, or spreadsheet |
| Post-mortems | Team documentation system (issue tracker, document, or wiki) |
| Customer notifications | Recorded in the Incident Log |

## Continuous controls

Always-on. Verified during annual reviews.

### Identity and access
- Individual accounts per system; federated to the corporate identity provider where supported, standalone with Password Policy otherwise
- MFA on source control, cloud-platform consoles, identity-provider admin consoles, and any system holding Confidential/Restricted data
- Federation is partial deprovisioning - Offboarding Checklist enumerates every vendor regardless of auth method
- No shared accounts (human or automated)
- Secrets in the secrets manager; never in source code, env files, or repos
- Auth exceptions documented in the ACM Exceptions column with compensating controls
- MFA via authenticator app or hardware key

### Code and infrastructure
- `main` branch-protected; direct pushes prohibited
- Every production change via PR with peer review
- CI must pass before deploy: tests + dependency scan + build
- Infrastructure managed as code
- Every deployment supports rollback

### Data and encryption
- TLS 1.2+ on public endpoints; HTTP redirects to HTTPS
- TLS 1.2+ on data-store and third-party connections
- Encryption-at-rest on every managed data platform
- Customer AI conversation data classified Restricted

### Devices
- All company devices in MDM
- MDM enforces FDE, 15-min auto-lock, remote wipe, auto OS updates
- Managed EDR running on every device
- BYOD not permitted for Restricted data without {{SECURITY_OWNER}} approval

### External communication
- Public security page live; {{SECURITY_CONTACT}} monitored
- External reports triaged through standard IR process

### Monitoring
- Alerts on IAM/policy changes, production config changes, authentication anomalies
- Logs retained for access and changes to production systems (≥ 90 days)
