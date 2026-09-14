# Draft-report QA: two lenses

Before a draft SOC 2 report is finalized, review it through **two lenses at once**:

- **(A) Auditor-QA** — genuine defects to send back to the auditor before signature.
- **(B) Customer's-eye** — how a buyer's vendor-risk team will read the finalized report, and where they'll
  push. This is the mirror image of the `vendor-risk-analyzer` skill; borrow its threat framing here.

Sort findings by whether they're **customer-visible / genuine correctness issues** (fix) vs **things only
you notice that add Type 2 burden** (leave). "Let sleeping dogs lie" is a real principle: don't wake a dog
that isn't barking.

## Know whose work is whose (don't over-improve the auditor's report)

- **The auditor owns the opinion and their standardized control catalog.** You flag material defects; you
  do **not** line-edit their boilerplate. Their control descriptions are written generically on purpose and
  are consistent across clients — bespoke language is *harder* for them to stand behind, not easier.
- **Management owns the system description and assertion** (the auditor reviews them). Description fixes are
  legitimately the client's to make.
- **You (advisor)** spot the genuinely material issues and prepare the client for how the report lands.

## The boilerplate-vs-real-contradiction line (important)

Do not mistake the auditor's generic control language for a false claim.

- **"Encryption technologies such as VPN, TLS, and SFTP are used"** — "such as" is an *illustrative,
  non-exhaustive* list. It is **not** an assertion the company runs a VPN. If the system description says
  "no VPN," that's not a contradiction — the specifics live in the description; the control is boilerplate.
  Don't send it back.
- Generic functional roles ("IT / HR / Help Desk"), "or equivalent," "at least annually" (a floor) — all
  read loosely and map to whoever/whatever performs the function. In an 8-person company the COO *is*
  "IT/HR" and GitHub *is* the "ticket/tracking system." Leave them.
- **What *is* worth flagging** is a genuine structural defect, not house style (next section).

## Auditor-QA checklist (genuine defects)

1. **Type 1 / Type 2 contamination.** A point-in-time Type 1 should carry **no** operating-effectiveness
   language. Flag: a "Results of Service Auditor **Test of Controls**" column; "throughout the **period**"
   on a single-date report; "**no events to test … during the review period**" notes. These are Type 2
   template artifacts and they contradict the opinion's own "we did not perform any procedures regarding
   operating effectiveness." This is a real coherence defect a sharp reader catches — worth a one-line
   heads-up to the auditor. (The correct Type 1 treatment is design-assessment language only; for a
   not-yet-exercised control, "newly implemented," not "no events during the period.")
2. **Subservice scope — run the two-question test, don't guess by "where data lives."** For each material
   provider ask separately: is it a *privacy subprocessor* (processes personal/customer data → DPA +
   accurate data terms), and is it a *subservice organization* whose controls your design relies on and
   excludes from the description (→ carve-out + CSOC tables + annual attestation review)? A provider can be
   both. The gap a reviewer most often raises: data stores holding customer data or credentials that are
   neither carved out nor otherwise addressed. For a model/inference provider, state the *actual* data terms
   (no-training / zero-or-limited retention), never "standard data handling terms" — a reviewer picks at
   "standard" immediately.
3. **Controls the narrative claims but the matrix omits.** For a Security report, conspicuous absences:
   an explicit **MFA** control, **encryption at rest**, **tenant isolation** (if single-tenancy is the
   headline claim). Note these — but as "the description commits to this; is there a mapped control?",
   which is a scope question for the auditor + management, not a rewrite you perform.
4. **Governance/filler mappings.** A vague "commitment to integrity" control reused across CC1 criteria,
   or mapped to board-independence (CC1.2) at a founder-run company, is the weakest spot. Better: state
   that board-level oversight is performed by management/founders given company stage.
5. **System-description accuracy** (management's to fix): mislabeled tech (e.g. "gRPC" as a "web app
   framework"), single-tenant-*compute*-over-shared-*data-layer* ambiguity when "single-tenant" is
   claimed, typos, and any stack detail (IdP, auth combo) that should be confirmed as accurate.

## Customer's-eye lens (lens B)

What a competent vendor-risk reviewer concludes and where they push:

- **Scope discounts they'll apply:** Type 1 (design on one day, not operating effectiveness) → "when is the
  Type 2, what period, is there a bridge letter?"; Security-only → for a product in the runtime path of
  customer data, expect **Confidentiality** and likely **Availability** as Type 2 asks; newly-implemented
  program → fine *if* framed as "not yet due," worse if framed as "no evidence."
- **Product-specific threat scenarios** (pick from the vendor-risk-analyzer breach patterns; the
  highest-weight ones recur):
  - **Credential-aggregation target.** A product that stores *customers'* read-only integration
    credentials across cloud/repo/SaaS/model becomes a single point whose compromise yields recon/access
    across the whole customer base. Read-only is not low-impact at that span. This is usually the
    top-weight scenario; it should drive both the client's roadmap (secrets vaulting, rotation, scoped
    permissions) and the answers they give buyers.
  - **Sensitive data through telemetry/observability/model pipeline.** Prompts/completions that "may
    contain sensitive data" landing in analytics stores and a third-party model provider under unstated
    terms.
  - **Subprocessor exposure of out-of-scope data stores** (ties to auditor-QA #2).
  - **Tenant-isolation failure** (IDOR/broken tenant check), made hard to refute proactively by the
    absence of a **penetration test**.
  - **Compromised engineer → push-to-prod** — usually where the SDLC controls are genuinely strong
    (branch protection, mandatory review, separate environments); a place to answer with confidence.
- **What to lead with (sales enablement):** the honest, specific system description; real structural SDLC
  controls; sensible access posture; encryption. Then have crisp answers ready for the predictable five:
  Type 2 timeline; why Security-only; how the out-of-scope data stores are governed; exactly what
  "single-tenant" means; and how the integration-credential store is protected.

## Program improvements to have in motion (not Type 1 blockers)

Penetration test (annual, scoped to API + tenant boundary — its absence is the most common single
follow-up); formal secrets management; EDR beyond AV on engineer endpoints; a log/redaction policy for what
may reach analytics/model providers; and planning Confidentiality (+ likely Availability) into the Type 2.
