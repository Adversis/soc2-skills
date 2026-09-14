# Policy Altitude: the lean policy/procedure doctrine

The single highest-leverage move in a SOC 2 engagement is getting the **policy set** right. GRC
platforms (Secureframe, Vanta, Drata) auto-generate ~21 policies (~100+ pages) from generic enterprise
templates. They assume a security team, a change advisory board, a formal ITSM ticketing system, and HR
functions a small company doesn't have. Policies that describe processes you don't run are **worse than
no policies**: they create audit findings, legal liability, and confusion about what the company does.

Replace the templated set with a lean, principles-based one (~15–17 policies, ~40 pages). A good public
starting base is Tailscale's open-sourced policy set (MIT, principles-based, GitHub/CI-native). Adapt to
the company's stack; add the few SOC 2 requires that a startup base omits (Physical Security, Encryption,
Acceptable Use).

## The core rule: policies name controls, procedures name products

**Policies state *what* control is in place and *what outcome* it achieves. Procedures (checklists,
runbooks, registers) state *how* it's implemented and *which tools* are used.**

Why: if a policy says "Okta is our identity provider" and the company moves to Entra, the policy needs a
formal revision cycle (review, approval, version bump) for zero security change. If the policy says "a
centralized identity provider is used" and the *onboarding checklist* says "provision an Okta account,"
only the checklist changes. Checklists don't go through policy review.

| Lives in policy | Lives in procedures |
|---|---|
| "Full-disk encryption is enforced by MDM" | "Enable FileVault via Mosyle" |
| "Secrets are stored in a secrets-management system" | "Use GCP Secret Manager; path in the password manager" |
| "All critical data stores are backed up daily" | "a managed Postgres: continuous backup, 7-day retention…" |
| "SSO is required for all systems that support it" | "Provision the IdP account; add to GitHub org via SSO" |
| "Infrastructure is managed as code" | "Terraform; state in GCS bucket X" |

**The deliberate exception — name the evidence-trail tool.** GitHub *is* named in policy for branch
protection / PR review / source control, because for code changes the tool *is* the control and PRs *are*
the change records. Naming it makes the evidence path explicit. Keep everything whose product identity is
incidental (incident tracking, logging store) generic — "issue tracker, document, or spreadsheet" — so
you don't hand the auditor a hostage.

This maps directly to what may / may not name a vendor: **policies use generic functional descriptions;
procedures and the system description may name specific systems.**

## Lean over comprehensive

Every specific tool, timeline, frequency, and named role in a policy is something an auditor can and will
test. A 5-person company with 107 pages of enterprise policy and no change-advisory-board minutes fails
more controls than one with 40 pages of honest policy and clean evidence behind each line.

- Keep policies at **policy altitude** — what is maintained/done and when, not the implementation
  mechanics, tool names, or architecture. Don't volunteer tool limitations. Don't restate rationale a
  policy doesn't need. Don't duplicate a field across sections (drift between them = a finding).
- Match the doc to reality: a 3-line checklist, not a 10-step one for a process nobody runs that way.
- **The answer to "why don't you have X?" beats a policy that describes X with no evidence of X.**

## Fold these in; don't write standalone policies

At small scale these standalone policies add ceremony without coverage — the controls are satisfied
elsewhere:

- **SDLC** → Change Management + Testing (PR review, branch protection, CI, security review for features).
- **Asset Management** → Access Control (service accounts, API keys) + Physical Security (devices).
- **Logging & Monitoring** → Risk Assessment + Patch Management + Incident Response. A standalone one
  tends to over-commit on retention periods / alert thresholds / SIEM tooling you can't evidence.
- **Vulnerability Management** → Patch Management.
- **Third-Party Risk** → Vendor Management.

## Common template-bloat fixes (recurring across engagements)

- **Physical Security** for a remote company: the template describes badge readers, visitor logs,
  generators, on-prem servers — none exist. Replace with one paragraph: fully remote, no facilities;
  physical data-center security is the cloud provider's (a carved-out subservice org); employees secure
  their devices per Acceptable Use; lost/stolen devices reported for remote wipe. Covers CC6.4, commits
  to nothing you don't operate.
- **Out-of-scope TSC policies** (Processing Integrity, Privacy) when scope is Security-only: **remove**
  from the audit package. Including them — often with foreign placeholders like "daily sync" or
  "Customer Success Manager" — creates risk for controls you never intended to be tested.
- **MFA language**: change "should be used" → "is required / enforced." Auditors check enforcement config,
  not verbs.
- **"SIEM tool(s)"** at a company that runs GCP Cloud Logging + Monitoring: name the actual tools or say
  "logging and alerting solutions." Don't imply a SIEM you don't have.
- **"The author of a change shouldn't deploy it"**: infeasible at 5 engineers and a routine violation as
  written. Replace with the compensating control: "When one engineer authors and deploys, a second
  engineer's PR approval is required before merge; emergency changes require CTO approval and post-hoc
  review."
- **Unfilled placeholders** ("maintained by ."): a dead giveaway the policy was never operationalized.
  Fill every owner/contact/RTO/RPO field before anything reaches the auditor.

## Calibrated commitments (and why the "worse" number is right)

Under-promise on paper; the tighter-sounding number just manufactures findings.

**Order of operations: configure the setting, capture dated evidence, then write the number into policy —
never the reverse.** Writing "15-character minimum" first commits you to a config you may not be able to
make; setting it in the IdP first, screenshotting it, then writing the matching number guarantees the
policy and the evidence agree.

- **Offboarding — 1 business day.** CC6.3 is among the most-tested. "Immediately" creates a finding on a
  weekend departure; 1 business day is the tightest defensible commitment without same-day automation.
- **Password length — 15 chars, no complexity rules.** NIST SP 800-63B: length over complexity;
  composition rules push users to `P@ssw0rd`.
- **Patch timelines — three-factor matrix** (CVSS severity × exploit availability [CISA KEV / public PoC]
  × exposure [internet-facing vs internal]). NIST 800-40r4 prioritizes exploitability and exposure over
  score. Gives the CTO documented discretion (e.g. a generous 365-day target for internal-only,
  no-known-exploit, medium) while committing to aggressive (14-day) timelines for actively-exploited.
  Run timelines from **confirmed presence in a used component**, not CVE publication date.
- **Breach notification — separate the regulatory clock from the customer one.** GDPR Art. 33 is
  *authority* notification (72 hours where required; processors notify controllers without undue delay) —
  it is not a rule to notify customers in 72 hours. Customer notification is a *contractual* commitment:
  pick an SLA (72 hours is a defensible default) and state it as contractual, not "because GDPR requires it."
- **RTO / RPO — 24 hours.** Conservative and honest for a team without dedicated ops; note the policy will
  be revisited if a contract requires tighter — a documented upgrade path, not a current overcommitment.
- **Annual cadence** for risk assessment, access review, vendor review. Quarterly is better practice but
  4× the evidence burden; annual is auditor-acceptable and operationally real. If you *say* quarterly you
  must *have* four documented cycles — say what you actually do.
