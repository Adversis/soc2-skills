# Auditor Q&A: the evidence-request loop and reusable responses

During a Type 1, the auditor works a predictable loop for each control:

1. "Provide **one example** of [the control operating]."
2. If you say it hasn't operated: "Provide supporting documentation confirming it **did not occur** as of
   [as-of date] (system logs, inventory, dashboards, or equivalent)."

So for every control you need either a **corroborating artifact** (it ran) or **proof of non-occurrence**
(it legitimately hasn't). Pre-stage both. The patterns below recur on nearly every engagement.

## Non-occurrence controls — supply evidence of the negative

Terminations / access revocation, internal transfers / access-change approvals, device disposals, security
incidents. Response shape:

> "No [terminations / transfers / disposals / incidents] occurred on or before [as-of date]. The
> [offboarding / access-change / disposal / IR] control is designed and implemented and will be evidenced
> at the first [event]. Attached: [the artifact proving zero] — e.g. the device-disposal log with no
> entries plus the asset inventory showing all devices active; the access matrix showing every employee in
> their hire-date role with no changes; the GitHub/GCP dashboard showing no open security incidents."

The negative evidence is the part teams forget. A log/inventory/dashboard *showing zero* is what closes it.

## Not-yet-due periodic controls — supply the schedule

Access review, performance review, BC/DR tabletop. Response shape:

> "The annual [X] had not yet come due as of [as-of date]; it is scheduled for [date]. See [screenshot of
> the scheduled task / calendar entry]."

## Onboarding and policy acknowledgment — produce the real records

If the company onboarded anyone before the as-of date, these records exist — provide the completed onboarding
access-approval record for a named hire, dated on/before the as-of date. If no hire occurred after the
control took effect, don't fabricate one: evidence the population (current roster/access) and note the
control is implemented but not yet exercised.

Nuance worth stating to the auditor: assign the **behavioral** policy set (Acceptable Use, Code of Conduct,
Data Classification, Personnel/Information Security) to everyone at onboarding and capture that
acknowledgment; assign **operational** policies to the roles that operate them on an annual cycle. It's
theater to pretend every operational policy was read and understood on day one — and the auditor accepts
the behavioral-vs-operational split when you state it plainly.

## Background checks — state what you actually do

If the company doesn't run third-party criminal checks, don't imply it does. State the real practice as
"screening performed per applicable employment requirements and company practice" (e.g., identity/work-
authorization and reference checks), and reconcile it with any system-description claim so the two don't
contradict. Do **not** make a specific program the security-control gate on system access — in particular
E-Verify can't be used for pre-employment screening and its case is opened *after* the I-9, no later than
the third business day of employment, so "E-Verify before access is granted" is both inaccurate and a
needless commitment. Keep the control at "eligibility/screening checks completed per policy."

## External vulnerability scanning — name it accurately, don't mislabel code scanning

SCA (dependency) and SAST (code) scanning scan your *code*, not your running attack surface; "external
vulnerability scanning" means DAST / infrastructure scanning of external-facing systems. If the company
doesn't run recurring external DAST, say so plainly — you don't need to dress up CI scanning as an external
scan, and a pen test isn't required for a Type I either. State it as: "We run dependency and static-analysis
scanning in CI plus cloud-security controls, and perform the security evaluations appropriate to our risk
assessment; recurring external DAST is not yet in place and will be added as the program matures or customer
demand warrants." Use the CI scanning evidence for "vulnerabilities identified and tracked to remediation,"
and record the external-DAST choice as a documented scope/compensating-control decision — don't claim code
scanning *is* infra scanning.

## Password policy — configure first, then screenshot

If asked for the password minimum, set the IdP/Workspace config to the committed length (12/15+) **first**,
then capture the screen dated on/before the as-of date. Evidence follows the real config, never the other
way around.

## The through-line

The auditor is confirming two things only: for "implemented," a real corroborating artifact; for "no
instances," proof of the negative. Give them exactly that, dated correctly, framed honestly. Everything
else is noise.
