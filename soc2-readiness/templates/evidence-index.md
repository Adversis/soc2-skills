# Evidence Index (template)

Build this **as you build controls**, not after. One row per control the auditor will test. The point is
that every row resolves to either a dated corroborating artifact or a dated proof-of-non-occurrence — and
you know which before the auditor asks.

## Status legend

- **In place** — control operated; a dated artifact exists (screenshot, ticket, completed checklist).
- **Not yet due** — periodic control, cycle scheduled after the as-of date; evidence = the schedule.
- **No instances** — event-driven control that legitimately hasn't triggered; evidence = proof of the
  negative (log/inventory/dashboard showing zero).
- **Compensating control** — the literal control isn't run; a documented alternative + scope decision
  covers it (e.g. external DAST deferred, SCA/SAST + peer review accepted).

## Index

| Control | Criterion (CCx.x) | Owner | Evidence artifact (self-documenting source preferred) | Artifact date (≤ as-of) | Status | Response framing / notes |
|---|---|---|---|---|---|---|
| MFA enforced on IdP / source control / cloud console | CC6.1 | | IdP MFA policy screenshot | | In place | Configure first, then capture |
| Logical access provisioning (onboarding approval) | CC6.2 | | Onboarding checklist / approval ticket for [hire] | | In place / No instances | If hires occurred, a real record; if none since the control took effect, evidence the population (roster/current access) |
| Access removal (offboarding) | CC6.3 | | Offboarding checklist | | No instances | "No terminations as of [date]; evidenced at first departure" + roster showing no departures |
| Periodic access review | CC6.3 | | Scheduled review task | | Not yet due | "Scheduled [date]" |
| Encryption in transit (TLS 1.2+) | CC6.7 | | TLS config / scan | | In place | |
| Encryption at rest | CC6.1/6.7 | | KMS / managed-DB encryption screenshot | | In place | |
| Change management (reviewed PR + branch protection) | CC8.1 | | GitHub PR + branch-protection settings | | In place | Tool named on purpose — PRs are the records |
| Vulnerability identification & tracking | CC7.1 | | Dependabot / CodeQL alerts (open/closed) | | In place | SCA/SAST = code, not external surface |
| External vulnerability scanning | CC7.1 | | Cloud SCC posture + scope decision | | Compensating control | External DAST deferred + accepted |
| Security incident response | CC7.3 | | Security dashboard (no open incidents) | | No instances | "No incidents as of [date]" + dashboard |
| Vendor / subservice review | CC9.2 | | Current SOC 2s on file + documented review | | In place | Review evidence within 30 days of each report |
| Risk assessment (annual) | CC3.x | | Risk register (completed assessment) | | In place | Perform before the Type I date — don't play "not yet due" here; date it within the window for Type II |
| BC/DR tabletop (annual) | CC7.5/CC9.1 | | Scheduled exercise | | Not yet due | "Scheduled [date]" |
| Performance / security review (annual) | CC1.5 | | Scheduled review | | Not yet due | "Scheduled [date]" |
| Device disposal | CC6.5 | | Disposal log (no entries) + asset inventory | | No instances | Reset-and-reassign practice, if that's the reality |
| Background screening | CC1.1/1.4 | | I-9/E-Verify + reference-check records | | In place | State actual practice; don't imply criminal checks |
| Policy acknowledgment | CC1.1/CC2.2 | | GRC acknowledgment records | | In place | Behavioral set at onboarding; operational set annually by role |

Add/remove rows to match the actual control set. Keep it in the engagement folder, updated live.
