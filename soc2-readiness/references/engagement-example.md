# Worked example: the ACME engagement

A real end-to-end Type 1 engagement this skill was distilled from. Names kept for the advisor's own
reference; the transferable parts are the decisions and numbers.

## The client, in one paragraph

ACME AI — a ~1-year-old, ~8-person cloud-native startup selling an AI-powered customer-support assistant for enterprise support teams
(read-only connectors that pull helpdesk conversations and knowledge content, a hosted LLM that drafts and summarizes replies, and dashboards on response quality). Stack: GCP/GKE, Cloud SQL for
PostgreSQL (config data + **customer integration credentials** + tenant metadata), BigQuery (customer
content + telemetry — may contain sensitive data), a hosted LLM provider (content routed
for inference), GitHub + Terraform (CI/CD), Google Workspace federated identity, Auth0
for end-user auth. GRC platform: Secureframe. Auditor: an AICPA-licensed CPA firm. Scope: **Type 1, Security TSC
only, as of June 8 2026.** Key framing: the *company* is a year old, but the *SOC 2 program* is newly
stood up — so periodic controls are "newly implemented, not yet due," never "young company."

## What was done (the arc)

- **Scoping & confirmation.** Security-only, Type 1 first. A confirmation pass surfaced the cost of
  *not* nailing the stack early — an early note assumed Okta as the IdP; the real answer was Google
  Workspace + Auth0. Confirm compute layer, IdP, access-review cadence, RTO/RPO, contractors, pen-test
  intent, and what each integration touches *before* writing.
- **Policies replaced, not filled in.** Secureframe's 21 policies (~107 pp, ~26,600 words) → a lean set
  of **17 policies (~40 pp, ~8,000 words)** adapted from Tailscale's open set. Every TSC stayed covered;
  procedural commitments that didn't match reality were removed. A full crosswalk mapped each old policy
  to its new home.
- **Procedures & evidence built together.** On/offboarding checklists, access-control matrices, risk /
  vendor / disposal / vuln registers, backup-recovery + IR runbooks and templates — each control wired to
  a self-documenting source (GitHub Security, GCP alert policies, the Secureframe agent inventory).
- **Vendor & subservice reviews.** Per-vendor reviews + CUEC tracking for GCP, Cloud SQL, BigQuery, the LLM provider,
  GitHub, Google Workspace, Microsoft/Azure, Slack, Secureframe; their SOC 2s collected on file.
- **Evidence & auditor Q&A.** Worked the auditor's "one example / now prove it didn't happen" loop across
  ~15 controls with the patterns in `auditor-qa-patterns.md` — non-occurrence for terminations /
  transfers / disposals / incidents, "not yet due" for access/perf/BCDR reviews, real records for
  onboarding, actual practice for background checks, and a documented compensating-control decision for
  external scanning.
- **Draft-report QA (two lenses).** A pre-finalization review of the auditor's draft.

## Decisions worth reusing

- **Calibrated commitments actually chosen:** offboarding 1 business day; password 15 chars; risk-based
  patch matrix (severity × KEV × exposure, 14-day for actively-exploited → 365-day for internal/no-exploit);
  72-hour breach notification; 24-hour RTO/RPO; annual cadences.
- **Folded in** (no standalone policy): SDLC → Change Mgmt + Testing; Asset → Access + Physical; Logging →
  Risk + Patch + IR; Vuln → Patch; Third-Party → Vendor. **Removed** out-of-scope Processing Integrity and
  Privacy from the audit package. **Rewrote** Physical Security to one cloud-native paragraph.
- **Scope discipline (the "is Linear in scope?" call):** default light — an internal work tracker is in the
  access/vendor inventory only, *not* the control layer, unless a control's evidence actually lives there.
  Pulling it into policies/system-description would have created evidence with no benefit.
- **Declined the pentest (CC7.1)** as "not now," backed by compensating controls (SAST + dependency
  scanning + scheduled external scans + merge review) and the arguable-criterion reading — with the rigor
  price paid (methodology, scope, schedule, tracked remediation). See `execution-playbook.md`.
- **Client-side config was the whole critical path.** Advisor work stayed on schedule; GCP/GitHub/endpoint
  config drove every slip. The date slipped twice (5/25 → 6/8), made early and cheaply. The fix was
  precision: per-repo, per-setting asks instead of "what are your CI job names."
- **Worked the Secureframe scoping tail** (override vs disable-test vs N/A) and stopped optimizing the red
  dashboard — auditors score controls, not the dashboard.

## The draft-review, and the calibration that matters most

The first-pass review flagged: (P0) Type 1/Type 2 template contamination in Section IV; (P0) subservice
scope covering only GCP while data lives in Cloud SQL/BigQuery/the LLM provider; (P1) boilerplate controls "contradicting"
the narrative; (P1) missing MFA / encryption-at-rest / tenant-isolation controls; plus governance/typo items.

Then the recalibration — **this is the reusable lesson**: most of that was over-reach. "Encryption
technologies **such as** VPN, TLS, and SFTP" is illustrative boilerplate, not a claim the company runs a
VPN — not a contradiction, and not the advisor's to rewrite. The auditor's generic control catalog is
theirs; you flag *material* defects, you don't line-edit house style. What genuinely survived: the Type 1/2
contamination as a one-line flag to the auditor; subservice scope as an awareness/scoping item; missing
controls as "the description commits to this — is there a mapped control?" And the durable value was **lens
B** — how the finalized report lands with a buyer's vendor-risk team, with the **credential-aggregation**
scenario (ACME stores customers' cross-provider read-only creds) as the highest-weight threat. See
`threat-lens.md` for the generalized version.

## Faster next time

1. Start from the lean principles-based base; never fill in a GRC platform's 21 templates.
2. Run the CTO confirmation pass before writing a word — it's the cheapest de-risking available.
3. Set "policies name controls, procedures name products" before authoring.
4. Build the evidence index alongside the controls; date everything ≤ as-of.
5. Pre-stage the auditor Q&A responses; you already know what they'll ask.
6. Reserve a two-lens draft review — and hold the boilerplate-vs-real line so you don't over-improve the
   auditor's report.

## One open question, held at low confidence

Part of the "assert, don't manufacture" reasoning rested on a belief that you can't really *fail* a SOC 2
— that unmet items surface as notes with little deal impact. That belief was **low-confidence and
load-bearing.** Qualified opinions exist; the reader who matters is the prospect's security reviewer, not
the auditor; where exactly the line sits (a control count, a specific control, or only a qualified
opinion) is unresolved. Don't inherit "you can't fail" as settled, and don't treat every red control as
existential either. Worth resolving with someone who has seen buyers reject a report.
