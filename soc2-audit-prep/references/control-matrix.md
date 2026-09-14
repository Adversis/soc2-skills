# The control matrix (the center of gravity)

The auditor evaluates *controls*, not policies. The control matrix is the one artifact where every control
the auditor tests is stated, owned, and tied to dated evidence. Building it well is the single
highest-value task of audit prep.

**The template (`templates/control-matrix.csv`) is a starter set spanning CC1–CC9, not an exhaustive or
sufficient list.** It shows the shape and a realistic baseline for a small SaaS Security-only Type I
(~35 controls covering governance, communication, risk, monitoring, control activities, access/encryption,
detection, incident response and recovery, change, and vendor/BCP). Tailor it: add, remove, and reword so
every in-scope Common Criterion is addressed by controls the company actually runs. A few dozen genuine
controls is the right size — not a dozen, and not hundreds.

## Columns

One row per control:

| Column | What goes in it |
|---|---|
| Control ID | Stable short id (AC-01, CM-01, IR-01…). Reference it from the system description and the policy. |
| TSC criterion | The Common Criteria point(s) the control addresses (CC6.1, CC8.1…). |
| Control statement | What the control *does*, in one sentence — the real mechanism, not boilerplate. |
| Owner | The role accountable (not a person's name in the template). |
| Frequency / trigger | Continuous, per-event (hire, departure, change, incident), or a periodic cadence + date. |
| System / population | Where it operates and what it applies to (the population the auditor will sample in a Type II). |
| Evidence | The specific corroborating artifact (config screenshot, ticket, completed checklist, dashboard). |
| Evidence date | On or before the as-of date. Blank is a gap to close. |
| Status | In place · Not yet due · No instances · Compensating control (see below). |

## Status vocabulary

- **In place** — control operated; a dated artifact exists.
- **Not yet due** — periodic control scheduled after the as-of date; evidence is the schedule (use the
  five-part shape from `soc2-readiness`'s `type1-evidence.md`).
- **No instances** — event-driven control that legitimately hasn't fired; evidence is proof of the zero
  population (a log/inventory/dashboard showing zero), not silence.
- **Compensating control** — the literal control isn't run; a documented alternative + scope decision
  covers the criterion (e.g. external DAST deferred, CI dependency + static analysis + peer review
  accepted).

## The reconciliation rule

This matrix is not the `00-meta` TSC→policy table (that maps *criteria to policies*, coarse) and not the
sample `evidence-index` (a starter grid). It maps **specific implemented controls to criteria**, and it is
the spine the rest of the submission hangs on:

- **Every affirmative claim in the system description reconciles to a matrix row.** If the description says
  "MFA is required for source control, the cloud console, and any system with customer data," there is a
  matrix row for it with dated evidence. If a description claim has no control + evidence behind it, either
  add the control or cut the claim (see `system-description-walk.md`).
- **Every policy commitment that is a testable control appears in the matrix.** A commitment with no matrix
  row is a claim you're not evidencing.
- **No orphan rows.** A matrix control that nothing in the description or policies supports is either
  removed or reconciled.

## How to build it

1. Start from the criteria in scope (Security / Common Criteria for a first Type I) and the policies +
   system description you already have.
2. For each criterion, write the *real* control(s) — the mechanism the company actually runs. Prefer
   automated, self-documenting controls; name the evidence source.
3. Assign the owner, frequency/trigger, and population.
4. Fill evidence + evidence date from the evidence repository (`evidence-repository.md`). Where a cell is
   blank, that's the punch list.
5. Walk the system description against the finished matrix (`system-description-walk.md`) and reconcile
   both directions until there are no orphan claims and no orphan rows.

Keep the matrix lean — it should have a row for each control that actually exists, not an aspirational
control set. Over-claiming here is what becomes a Type II exception when each row gets sampled.
