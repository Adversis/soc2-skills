---
name: soc2-readiness
description: >
  Advise a company through SOC 2 readiness and audit — the producer/advisor side (getting your own
  SOC 2), the mirror of vendor-risk-analyzer (evaluating someone else's). Use whenever the user is
  standing up a SOC 2 program, choosing scope (TSC / Type 1 vs Type 2 / as-of date / system boundary),
  writing or trimming security policies and procedures, deciding what is "in scope," selecting or
  carving out subservice organizations, building an evidence trail, drafting responses to an auditor's
  evidence requests, or doing a pre-finalization QA pass on a draft SOC 2 report. Trigger phrases:
  "SOC 2 readiness", "we're doing a SOC 2", "help us get SOC 2", "review our policies for SOC 2",
  "Type 1 vs Type 2", "auditor is asking for evidence of X", "what's in scope for SOC 2",
  "review this draft SOC 2 report", "Secureframe/Vanta/Drata policies", "prepare for the audit".
  Produces lean, honest, low-liability compliance work calibrated to a real company — not templated
  compliance theater. Complements the vendor-risk-analyzer skill (consumer side).
---

# SOC 2 Readiness (Advisor Side)

## Purpose

Help a company **earn** a defensible SOC 2 that a senior auditor signs cleanly and a sophisticated
buyer accepts — while creating the least possible future liability. This is the producer side of the
same coin as `vendor-risk-analyzer` (which reads a *finished* report as a customer). Reach for that
skill's threat framing when doing the customer's-eye QA lens (see `references/threat-lens.md`).

The governing ethic, distilled from a real engagement: **lean, honest, low-liability.** Every sentence
in a policy is a commitment an auditor can test; every control you claim in a Type 1 gets tested for
operating effectiveness in the Type 2. Under-promise on paper, over-deliver in evidence.

**The one pattern behind most decisions:** you write the policy, and the policy is what you're tested
against — so the leverage is *upstream*. Write commitments you can actually meet, and the downstream
answers become easy. The policy teardown, the calibrated numbers, the "not yet due" answers, declining a
control — these look like separate calls; they are one call made many times. The corollary is the failure
mode: **an unoperated policy is a stated commitment you are failing.** Heavy policies don't slow a startup
down — the startup ignores them, and now there's a self-inflicted finding. That's the reason to stay lean.

## When to use / not

Use for: scoping, policy/procedure authoring or trimming, "is X in scope," subservice vs subprocessor
decisions, evidence-trail design, drafting auditor responses, and draft-report QA.

Do **not** use this to: fabricate or backdate evidence (ever — see Guardrails); rewrite the *auditor's*
report or their standardized control catalog (that's their work product — you flag material defects,
you don't line-edit boilerplate); or pad a program to look more mature than it is (that backfires).

## The mental model (load this first)

- **SOC 2 is an opinion, not a grade.** A CPA issues unqualified (clean), qualified ("except for"),
  adverse, or disclaimer. Clean-with-exceptions is normal and fully usable. Adverse/disclaimer
  essentially only happen to the unprepared. Aim for clean; a single qualified area is survivable.
  **Open question — hold it at low confidence, don't treat it as settled:** *exactly* what "fails" a
  SOC 2 in a buyer's eyes, and what a prospect's security reviewer does with an exception, is not well
  established. Qualified opinions exist; reports do get rejected (~38% of orgs report one rejected). The
  unresolved part is where the line sits — a control count, a specific control, or only a qualified
  opinion that costs a deal. Do **not** conclude "you can't really fail," and do **not** treat every red
  control as existential. Both are wrong; the honest answer is unknown, and worth resolving with someone
  who has watched buyers reject reports.
- **Type 1 = a snapshot** of design + implementation *as of one date*. **Type 2 = a movie** of
  operating effectiveness *over a window* (3–12 mo, sampled ~25/population). Type 1 has little surface
  for exceptions; Type 2 is where they appear. **Do the Type 1 as a dress rehearsal**, then run the
  Type 2 window once the high-failure controls are automated.
- **Maturity, not size, drives outcomes.** A tight scope + automated, self-documenting controls beats
  a big manual program. Startups' advantage is scope: Security TSC only, production only, year one.
- **"In scope" = part of the service-delivery path, OR where a control operates and produces evidence.**
  An internal work tracker is neither by default. Keep it light: it's in the access/vendor inventory
  only, unless a control's evidence genuinely lives there (see `references/policy-altitude.md`).
- **Where exceptions actually come from** (rank is stable across auditors): CC6 access control (late
  offboarding + skipped/undocumented access reviews) is #1 by far, then CC8 change management, then
  CC9 vendor management, then CC3 risk assessment, then CC7 logging/monitoring/IR, then evidence gaps,
  then HR/encryption. Manual memory-dependent controls fail; automated self-documenting ones pass.

## The engagement, in phases

Run these roughly in order; earlier phases de-risk later ones. A worked example of the whole arc is in
`references/engagement-example.md` (an anonymized real Type I engagement).

1. **Scope & confirm the real stack.** Pick TSC (Security only unless a customer contractually requires
   more), Type 1 vs 2, the as-of date, and the system boundary. Then run a short **CTO confirmation
   pass** before writing anything: actual compute layer, identity provider, access-review cadence,
   RTO/RPO, contractors with prod access, pen-test intent, and exactly what customer data each
   integration touches. Unconfirmed assumptions here (e.g. guessing the IdP) cost the most downstream.
2. **Policies — replace, don't fill in.** GRC platforms (Secureframe/Vanta/Drata) auto-generate ~21
   bloated, templated policies. Replace them with a lean principles-based set (~15–17, Tailscale-style)
   calibrated to the company. The rule that governs everything: **policies name controls, procedures
   name products.** Full doctrine, the fold-in/leave-out list, and calibrated commitment defaults:
   `references/policy-altitude.md`. **Don't start from a blank page** — the entire refined set (policies,
   procedures, registers, system description) is in `templates/starter-pack/`, ACME'd with `{{TOKENS}}`;
   copy it and work through its `REPLACE.md`.
3. **Procedures & evidence infrastructure.** Checklists (on/offboarding), matrices (access control),
   registers (risk, vendor, disposal, vuln), runbooks (backup/DR, IR). Wire each control to a
   **self-documenting source** (GitHub Security, GCP alert policies, the GRC agent inventory) and start
   the evidence index (`templates/evidence-index.md`) *as you build*, not after.
4. **Vendor & subservice management.** Inventory vendors; collect critical vendors' current SOC 2s. For
   each material provider ask **two separate questions**, not an either/or: (a) is it a *privacy
   subprocessor* — it processes personal/customer data on your behalf → DPA + accurate data terms; and
   (b) is it a *subservice organization* your control design relies on — it performs services whose
   controls you depend on and exclude from your description → carve-out with CSOC tables + annual
   attestation review. A provider can be both, one, or neither. Getting this right is the #1 thing a
   customer's vendor-risk team checks.
5. **Evidence collection & auditor Q&A.** Every artifact dated **on or before the as-of date**; a policy
   alone is not "implemented" (the auditor needs a corroborating artifact). Pre-empt the auditor's
   "give me one example / now prove it *didn't* happen" loop with the response patterns in
   `references/auditor-qa-patterns.md`; use the five-part shape for not-yet-due controls and the
   backdating prohibition in `references/type1-evidence.md`. **Client-side platform config is the actual
   critical path** — not your writing — so issue config asks with per-repo, per-setting precision,
   calibrate the auditor early, and work the GRC platform's scoping tools: `references/execution-playbook.md`.
   Once the program is authored and you're finalizing for submission — the real control matrix, the
   system-description verification walk, the evidence repository, and the go-live gates — hand off to the
   **soc2-audit-prep** skill.
6. **Draft-report QA (two lenses).** Before finalization, review the auditor's draft as (A) auditor-QA
   — catch genuine defects to send back — and (B) how a buyer's vendor-risk team will read it. Full
   checklist, the boilerplate-vs-real-contradiction line, and the product-specific threat lens:
   `references/threat-lens.md`.
7. **Type 2 runway.** The whole game is the 60–90 days before the Type 2 window: automate deprovisioning
   (HRIS/SCIM→IdP); put every periodic control (access review, vendor review, risk assessment, BCDR
   tabletop) on a dated, owned recurring reminder in the client's system of record — the GRC platform's
   task scheduler if they have one (don't build a parallel one), a recurring ticket in their tracker if
   they don't — each producing a signed, dated artifact when it fires; enforce MFA; and lock change
   management to reviewed PRs + a written emergency-change procedure. You **cannot** retroactively fix an operating-effectiveness miss — only backfill docs for a
   control that *did* run. Don't start the clock until ready.

## Decision defaults (calibrated to a small, real company)

These are defensible starting points, not laws — each trades a better-sounding number for one that
won't create a finding. Rationale for each is in `references/policy-altitude.md`.

| Decision | Default | Why not the "better" number |
|---|---|---|
| TSC scope, year 1 | Security only | Extra categories = extra testable surface with no deal need yet |
| Report type | Type 1 first, then Type 2 | Dress rehearsal builds the discipline for a clean Type 2 |
| Offboarding SLA | 1 business day | "Immediately" creates findings on weekend departures |
| Password minimum | 15 chars, no complexity rules | NIST 800-63B: length beats complexity |
| Patch timelines | Risk-based matrix (severity × exploit × exposure) | CVSS-only is blunt; matrix gives documented discretion |
| Breach notification | 72 hours | Defensible contractual default — keep it contractual, not "because GDPR" (Art. 33 is authority notice, not customer) |
| RTO / RPO | 24 hours | 4-hour sounds better, fails on a real incident |
| Periodic cadence | Annual (risk assessment, access review, vendor review) | Quarterly = 4× the evidence burden; annual is auditor-acceptable |
| Policy count | ~15–17 lean policies | 21 templated policies = audit surface with no coverage gain |

## Guardrails (non-negotiable)

- **Never backdate or fabricate.** A backdated artifact makes the auditor's signed opinion false *on
  their license*. For periodic controls not yet exercised, the honest and sufficient framing is
  **"designed and implemented; first cycle scheduled for [date]"** — never "young company," and
  "no instances" *only where genuinely true*.
- **Lean beats comprehensive.** A process you describe but don't run is worse than nothing. The answer
  to "why don't you have X?" is better than a policy describing X with no evidence of X.
- **Respect the boundary of your role.** The auditor owns the opinion and their control catalog;
  management owns the system description and assertion; you (advisor) spot material issues and prep the
  client. Don't over-improve the auditor's work — flag defects, don't rewrite boilerplate.
- **Honest framing wins with senior reviewers.** "Newly implemented, cycles not yet due" reads far
  better than "no evidence yet," and is the same fact stated truthfully.

## Files in this skill

- `references/policy-altitude.md` — the policy/procedure doctrine, lean-set design, calibrated commitments.
- `references/type1-evidence.md` — Type 1 mechanics, as-of discipline, "not yet due" framing, no backdating.
- `references/auditor-qa-patterns.md` — the auditor evidence-request loop and reusable response patterns.
- `references/threat-lens.md` — draft-report QA (two lenses), boilerplate line, product threat scenarios.
- `references/execution-playbook.md` — running the engagement: client-config critical path, precision
  asks, calibrating the auditor, declining a control, GRC-platform scoping, IAM rightsizing, scheduling.
- `references/engagement-example.md` — an anonymized real Type I engagement, end to end, as a worked reference.
- `templates/evidence-index.md` — control → criterion → artifact → date → status, ready to copy.
- `templates/starter-pack/` — the full ACME'd document set (17 policies, procedures, registers, system
  description) with a `REPLACE.md` fill manifest; the fastest way to start a new engagement.
