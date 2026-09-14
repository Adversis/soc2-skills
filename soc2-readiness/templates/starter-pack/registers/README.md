# Registers

Records referenced by the policies and procedures.

## Live records you fill (ship header-only)

- `access-change-log.csv` — access grants, changes, revocations
- `device-disposal-log.csv` — device return / wipe / decommission
- `vulnerability-tracker.csv` — findings and remediation
- `risk-register.csv` — your live risk assessment. Ships header-only; populate it from
  `risk-register-SAMPLES.csv`, adapted to the client's real risks. One documented annual assessment is
  what CC3 wants.

## Reference content to adapt (not a live record)

- `risk-register-SAMPLES.csv` — reusable AI/cloud-startup risk scenarios (impact/likelihood/treatment) to
  copy into the live register and edit. These are example rows, several stated as concrete "facts" (clean
  vendor SOC 2s, a zero-retention model provider, phishing-resistant MFA) — verify or rewrite each before it
  becomes an assertion, and don't hand this file to the auditor as the assessment.

## Optional depth (not in the default deliverable)

The CUEC tracker lives at `../optional/cuec-tracker.csv`, deliberately outside the default pack.

- **It is optional depth, not a Type I requirement.** It aggregates the Complementary User Entity Controls
  each subservice org's SOC 2 places on you ("enforce MFA on your org," "require 2FA," "pin CI actions"),
  tracked to closure.
- **What CC9.2 actually requires:** a vendor inventory, risk-based tiering, and evidence you reviewed
  critical vendors' SOC 2 reports (review the opinion, exceptions, and coverage, and record the risk decision) at least annually, plus a
  DPA on file for processors of customer data. That is the whole bar.
- **The lighter default that meets it:** per material vendor, read the CUEC section of their SOC 2, act on
  the controls that matter (most already map onto your own CC6 controls — MFA, access, key handling), and
  document that you reviewed the report. Do **not** track a hundred-plus CUEC rows to closure for a first
  Type I.
- **Use the full tracker only when it will actually be worked** — a client who will act on it, a customer
  contractually demanding it, or a move toward regulated data. See the skill's `execution-playbook.md`,
  "Right-sizing vendor management."
