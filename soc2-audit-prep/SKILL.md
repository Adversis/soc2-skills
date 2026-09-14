---
name: soc2-audit-prep
description: >
  The finalization-and-handoff stage of a SOC 2 — turning an authored readiness program into the actual
  audit submission the auditor tests. Use when a company (usually the client/CTO with their advisor) is
  heading into fieldwork: building the real control matrix, verifying the system description claim-by-claim
  against evidence, collecting the evidence repository dated to the as-of date, running the go-live gates,
  and deciding what goes to the auditor vs. what stays advisor working papers. This is the stage AFTER
  soc2-readiness (which scopes and authors the lean program). Trigger phrases: "audit-ready", "auditor
  kickoff", "going to fieldwork", "build the control matrix", "control/evidence matrix", "what evidence
  does the auditor need", "prep for the SOC 2 auditor", "finalize the system description", "hand off to the
  auditor", "collect SOC 2 evidence", "as-of date evidence". Complements soc2-readiness (authoring side)
  and vendor-risk-analyzer (consumer side).
---

# SOC 2 Audit Prep (Finalization & Handoff)

## Purpose

Bridge the gap between "the program is authored" and "the submission the auditor can actually test." A
readiness pack (from `soc2-readiness`) is necessary but not sufficient: a nicely drafted policy does not
close the loop by itself. The auditor stops reading policies and says **"show me the control, and prove it
existed on the as-of date."** This skill is the work that answers that.

The consumer shifts here too. `soc2-readiness` is advisor-authoring; this stage is the **client/CTO plus
advisor** verifying reality and assembling the submission. The center of gravity moves from *policies* to
the **control matrix and the evidence behind each line**.

## Where this sits

`soc2-readiness` (scope + author the lean program) → **soc2-audit-prep** (finalize + evidence + hand off)
→ [auditor fieldwork] → draft-report QA (the two-lens review in `soc2-readiness`'s `threat-lens.md`).

## The mindset

- **A policy is a claim, not evidence.** For a Type I the auditor confirms *suitably designed AND
  implemented as of the date* — which means a corroborating artifact dated on or before the as-of date for
  every control, or proof of a zero population where a control legitimately hasn't fired.
- **The system description is the highest-liability document in the pack** — once management puts it in the
  report, management is asserting every affirmative sentence is true. The auditor tests what management
  describes. So each claim must be verified or cut before it ships.
- **Reduce contradictions, attach reality.** At this stage the highest-value work is *not* more SOC 2
  content or better policy prose — it's making one version of reality and putting evidence behind each
  control. Perfecting the policies further is wasted motion.

## The seven gates (the backbone)

Run these before anything goes to the auditor. None is optional; each closes a specific way the submission
fails on inspection.

1. **Strip the template.** Remove every `ACME` value, `{{TOKEN}}`, `.cmt` annotation, `[TEMPLATE NOTE …]`
   banner, and stale example from the deliverable versions. Advisor scaffolding must not reach the auditor.
2. **One version of reality.** Reconcile every remaining contradiction across policies, procedures, the
   system description, and the registers — classification vocabulary, retention periods, RTO/RPO scope,
   owner roles, offboarding population, screening and notification language. A reader who finds two answers
   files a finding.
3. **Accurate control language.** Confirm each control describes what the company *actually does* (not the
   boilerplate) — screening "per applicable employment requirements" not an E-Verify gate; customer
   notification as contractual, not "because GDPR"; change management as the real reviewed-PR flow.
4. **Build the real control matrix** — not the sample evidence-index. This is the center of gravity; see
   `references/control-matrix.md` and `templates/control-matrix.csv`.
5. **Walk the system description** with the CTO, line by line: verified / change / delete, evidence beside
   every material claim. See `references/system-description-walk.md`.
6. **Populate the evidence repository**, every artifact dated on or before the as-of date. See
   `references/evidence-repository.md`.
7. **Assemble the submission and apply hand-off hygiene** — the auditor gets management's audit documents;
   advisor working papers stay back (see below).

## Center of gravity: the real control matrix

Every control the auditor tests reconciles to one row:

`Control ID → TSC criterion → control statement → owner → frequency/trigger → system/population →
evidence → evidence date → status`.

This is a different artifact from `soc2-readiness`'s two coarser maps: the `00-meta` TSC→policy table
(coverage at the criterion level) and the sample `evidence-index` (a starter grid). The matrix maps
**specific implemented controls** to criteria, and **every affirmative claim in the system description must
reconcile to a row in it.** Full guidance: `references/control-matrix.md`.

## Hand-off hygiene: what the auditor gets vs. what stays back

Give the auditor management's audit documentation: the finalized system description, the policies, the
control matrix, and the evidence. Do **not** volunteer advisor working papers:

- the CUEC tracker (`optional/cuec-tracker.csv`) unless they ask;
- the `00-meta` advisor/adaptation notes and open-questions;
- the `soc2-readiness` engagement example, threat-lens, or auditor-Q&A guidance (these are how the advisor
  works, not management's records);
- any `.cmt` advisory annotations (gate 1 removes them).

## Guardrails

- **Never backdate or fabricate** — the brightest line in the engagement. A backdated artifact makes the
  auditor's signed opinion false on their license. For not-yet-due periodic controls, "designed and
  implemented; first cycle scheduled for [date]" is honest and sufficient; where a control legitimately
  hasn't fired, evidence the zero population — don't invent a run.
- **Insist on a "no surprises" auditor** who flags potential exceptions during fieldwork, not at draft.
- **For any exception, write a strong management response** — root cause, isolated vs. systemic,
  remediation, compensating controls.

## Files in this skill

- `references/control-matrix.md` — how to build the real control matrix; how it reconciles to the system
  description; the status vocabulary.
- `references/system-description-walk.md` — the line-by-line verified/change/delete walk, with the
  high-liability claims to circle.
- `references/evidence-repository.md` — the concrete evidence checklist, dated to the as-of date.
- `templates/control-matrix.csv` — a starter control set spanning CC1–CC9 to tailor (add/remove to cover
  every in-scope criterion), not an exhaustive list.
