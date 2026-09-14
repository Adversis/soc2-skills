# The system-description verification walk

The system description is the highest-liability document in the submission. Once management puts it in the
SOC report, management is asserting every affirmative sentence is true, and the auditor tests what
management describes. So before it ships, walk it line by line with the CTO and mark each material
statement **verified / change / delete**, with evidence beside every claim that survives.

## How to run the walk

Go sentence by sentence through the description. For each affirmative statement:

- **Verified** — it's true and there's a control + dated evidence behind it (a matrix row). Note the
  Control ID / evidence next to it.
- **Change** — it's overstated, imprecise, or aspirational. Reword to what is actually true today. ("All
  data is encrypted at rest with AES-256" → the real, verified scope.)
- **Delete** — it's not true, not evidenced, or not worth asserting. Cut it. An unprovable claim is pure
  downside.

A statement with no evidence and no matrix row cannot be "verified." Either build the control and evidence,
or change/delete the sentence.

## Claims to circle first (highest liability)

These are the sentences an auditor most often asks to prove. Don't assume — confirm each against reality:

- **Tenant isolation** — "each customer gets a dedicated single-tenant deployment." Confirm what is
  actually single-tenant: compute? database? cluster? If the data layer is shared with tenant scoping,
  "single-tenant" is an overclaim — describe the real isolation model precisely.
- **Encryption** — "all data encrypted in transit (TLS 1.2+) and at rest (AES-256)." Confirm the actual
  TLS floor and that at-rest encryption is on for every store named; scope the claim to what's true.
- **Alerting** — "automated alerts for IAM/config/authentication anomalies." Confirm the alerts exist and
  fire; keep a sample. Don't describe monitoring that isn't configured.
- **Logged privileged access** — "personnel access to configuration/customer data is logged and approved."
  Confirm the log exists and the approval step is real.
- **Screening** — describe screening "per applicable employment requirements and company practice"; don't
  assert I-9/E-Verify as a pre-access gate (E-Verify can't be used pre-employment).
- **Subservice orgs / subprocessors** — confirm each provider's treatment is right (run the two-question
  test from `soc2-readiness`'s `threat-lens.md`), the data terms are the *actual* terms (not "standard"),
  and the CSOCs/CUECs listed match the carve-out.
- **Model/inference provider retention** — state the real no-training / zero-or-limited-retention posture,
  confirmed against the provider's terms.
- **Backups / recovery** — confirm the backup cadence and that at least one restore test exists if the
  description claims recoverability is tested.

## Output of the walk

- A system description where every surviving sentence is true and traceable to a control + evidence.
- Matrix rows added or adjusted so the description and the control matrix reconcile in both directions
  (see `control-matrix.md`).
- A short list of claims changed or deleted, so the CTO has a record of what was cut and why.
