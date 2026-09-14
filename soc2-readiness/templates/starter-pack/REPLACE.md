# REPLACE — the fill manifest for the starter pack

Every client-specific value in this pack is either the sample company **ACME** (find/replace it) or a
`{{TOKEN}}` (fill it). Calibrated numbers (offboarding SLA, password length, patch timelines, RTO/RPO,
cadences) are **baked in as recommended defaults** — confirm them, don't assume they need changing.

## Step 1 — sample company name

Find/replace across the whole pack: **`ACME AI, Inc.`** → client legal name, **`ACME`** → short name.
ACME is used as a concrete sample company so the docs read like finished documents, not blanks.

## Step 2 — tokens

| Token | Meaning | Example | Lives in |
|---|---|---|---|
| `{{SECURITY_OWNER}}` | Role that owns the policy set and runs the reviews | CTO | policies + procedures |
| `{{SECURITY_CONTACT}}` | Monitored security address | security@acme.example | policies + procedures |
| `{{TEAM_SIZE}}` | Approx headcount phrase | "small (~8-person)" | meta, a few policies |
| `{{OWNER_EMAIL}}` | Risk/vendor register row owner | ops@acme.example | registers |
| `{{ADVISOR}}` | Advisor/firm that adapted the set (meta doc "Adapted by") | Adversis | meta doc |
| `{{EFFECTIVE_DATE}}` | Policy adoption date | 2026-05-07 | every policy header |
| `{{AS_OF_DATE}}` | SOC 2 Type I as-of date | 2026-06-08 | system description, evidence |
| `{{CUSTOMER_DATA_RETENTION}}` | Active-account retention period for customer data (set explicitly — never "indefinite") | e.g., 12 months | data retention, system desc |
| `{{IDP}}` | Corporate identity provider / SSO | Google Workspace (federated) | procedures |
| `{{VCS}}` | Version control + CI — **the one tool named in policy** (PRs are the change records) | GitHub | policies + procedures |
| `{{CLOUD_PROVIDER}}` | Primary cloud | GCP / GKE | procedures, system desc |
| `{{SECRETS_MANAGER}}` | Secrets store | GCP Secret Manager | procedures |
| `{{MDM}}` | Device management / endpoint | Mosyle (or Kandji/Jamf) | procedures |
| `{{PASSWORD_MANAGER}}` | Team password manager | 1Password / Bitwarden | procedures |
| `{{GRC_PLATFORM}}` | Compliance automation platform | Secureframe | meta, procedures |
| `{{DATA_STORE}}` | A managed data platform holding customer/config data (repeat per platform) | Cloud SQL (Postgres), BigQuery | procedures, system desc |
| `{{MODEL_PROVIDER}}` | LLM / inference subprocessor, if any | a hosted LLM provider | procedures, system desc |
| `{{APP_AUTH}}` | End-user auth for the product, if separate | Auth0 | procedures, system desc |
| `{{CHAT_PLATFORM}}` | Team chat | Slack | procedures |
| `{{CHAT_CHANNEL}}` | Channel for alerts/security | #dev | policies, procedures |
| `{{PRIVACY_URL}}` | Public privacy/data page | acme.example/privacy | data retention, system desc |
| `{{SECURITY_URL}}` | Public security page | acme.example/security | security-page, system desc |
| `{{OFFICE_RECEPTION}}` | Building/reception for badge access, or "N/A — fully remote" | Regus | offboarding, physical security |

## Step 3 — calibrated defaults (confirm; adjust only with reason)

These are the recommended numbers, not placeholders. Each trades a better-sounding value for one that
won't manufacture a finding (rationale: `../../references/policy-altitude.md`).

- Offboarding: **1 business day** · Password: **15 chars, no complexity rules**
- Patch matrix: **14 days** actively-exploited → **365 days** internal / no-known-exploit (severity × KEV × exposure)
- Breach notification: **72 hours** · RTO / RPO: **24 hours** · Periodic cadences: **annual**

## Step 4 — adapt to the real stack, then delete what doesn't apply

- Confirm the client's actual tools before filling tokens (run the CTO confirmation pass — see the skill's
  `SKILL.md` phase 1). Unconfirmed stack facts are the most expensive mistake.
- Delete sections that don't apply (no office → drop badge/reception lines; no model provider → drop
  `{{MODEL_PROVIDER}}` rows). A control you keep is a control you're tested on.
- Policies stay **vendor-neutral** — the only product named in a policy is `{{VCS}}`, deliberately.
  Procedures and the system description name tools. (This mirrors the "no vendor names in policies" rule.)
- The **risk register** ships as two files: `registers/risk-register.csv` header-only (the live register you
  fill) and `registers/risk-register-SAMPLES.csv` with reusable AI/cloud-startup scenarios to adapt into it
  (verify each sample "fact" before it becomes an assertion; one documented annual assessment is what CC3
  wants). The operational logs (access-change, device-disposal, vulnerability-tracker) ship **header-only** —
  fill as events occur.
- **`optional/cuec-tracker.csv` is optional depth, not a Type I requirement.** CC9.2 asks for a vendor inventory,
  risk tiering, and evidence you reviewed critical vendors' SOC 2s annually (plus a DPA for processors) — not
  a hundred-plus-row CUEC tracker. Lighter default: per material vendor, read the CUEC section, act on what
  matters (most map to your own CC6 controls), document the review. Use the full tracker only if the client
  will work it. See `registers/README.md` and the skill's `execution-playbook.md`.

## Step 5 — client-confirmation checklist (generalized from the source engagement's open questions)

Resolve each before the audit; each maps to a control an auditor will test:

1. **Background checks** — what's actually performed (identity/work-authorization + reference checks vs.
   third-party criminal)? State it as "per applicable employment requirements and company practice"; don't
   gate system access on E-Verify (it can't be used pre-employment).
2. **Security training** — platform and cadence; completion tracked where?
3. **Data retention** — customer-data retention period, aligned to `{{PRIVACY_URL}}` and contracts.
4. **Backup testing** — cadence + which systems, last test date, result (in the Backup Recovery Runbook).
5. **Offboarding coverage** — the checklist enumerates *every* system (no enterprise SSO/SCIM = manual).
6. **Encryption keys** — provider-managed sufficient, or any contract requiring customer-managed keys?
7. **Vendor register** — all material vendors present; subservice SOC 2s on file.
8. **Incident contact** — `{{SECURITY_CONTACT}}` exists and is monitored.
