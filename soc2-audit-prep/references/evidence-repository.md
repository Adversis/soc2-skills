# The evidence repository

Every control in the matrix resolves to a real artifact. Collect them into one place, each dated **on or
before the as-of date**, and link each to its matrix row. This is the punch list that turns a designed
program into an implemented one.

Prefer automated, self-documenting sources (the platform's own dashboards/config) over manual logs — they
carry their own dates and are what auditors trust.

## The checklist

Populations and rosters:
- Current employee/contractor roster (the population for access, training, screening).
- Vendor inventory with tiering; subservice orgs identified.
- Access population (who has access to what — the access-control matrix, current).

Access & identity (CC6):
- MFA enforcement config for source control, cloud console, and identity provider admin (screenshot).
- Onboarding access-approval record for a real hire if any occurred before the as-of date; if none since the control took effect, evidence the population (current roster/access) instead of fabricating one.
- Offboarding: a completed run if anyone departed; otherwise evidence of the zero population.
- Periodic access review: the schedule if not yet due, or a signed, dated review if performed.
- Password/authentication config (the committed minimum, screenshot — configured before captured).

Change management (CC8):
- Branch-protection settings on protected branches (screenshot).
- At least one real reviewed-and-approved pull request (the change-record evidence).
- Emergency-change procedure, and any emergency change documented retroactively.

Operations, monitoring, IR (CC7):
- Dependency/static-analysis scanning running in CI (alerts view, open/closed).
- Alerting configuration for the anomalies the description claims, plus a sample fired alert if available.
- Incident population: the incident log (empty is fine — but it must exist and be shown).
- Backup configuration for each critical store; at least one restore-test record if recoverability is
  claimed as tested.

Endpoints & assets (CC6):
- Device inventory; full-disk-encryption / screen-lock config; EDR/AV status; remote-wipe capability.

Governance & people (CC1–CC3):
- Risk assessment: one documented, dated assessment (the risk register populated from the samples).
- Security-awareness training completion per person; policy acknowledgments per person.
- Screening records per the real process (identity/work-authorization and reference checks).

Vendor & data (CC9, C-adjacent):
- Critical vendors' current SOC 2 reports on file; documented review of each (opinion, exceptions,
  coverage — with the risk decision recorded); DPAs for processors of customer data.
- Data-flow / system architecture evidence supporting the system description.

## Working rule

If an artifact won't exist by the as-of date, the control is "not yet due" or "no instances" (with proof of
the zero population) — never a backdated or invented artifact. A blank evidence cell in the matrix is the
work to do before fieldwork, not something to paper over.
