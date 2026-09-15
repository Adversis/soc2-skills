---
name: vendor-risk-analyzer
description: >
  Analyze vendor SOC 2 reports, MSAs, DPAs, and other security documentation for realistic risk assessment.
  Use this skill whenever the user uploads or pastes a SOC 2 report, SOC 2 Type I or Type II, 
  Master Service Agreement, Data Processing Agreement, Data Processing Addendum, BAA, vendor security questionnaire, 
  or any combination of these for vendor risk evaluation. Also trigger when the user asks about vendor risk management, 
  third-party risk assessment, or wants to evaluate whether a vendor is safe to integrate with. 
  Trigger on phrases like "analyze this SOC 2", "review this vendor", "is this vendor secure enough", 
  "vendor due diligence", "third-party risk", "should we integrate with", "evaluate this DPA/MSA/BAA".
  This skill produces a structured markdown report with realistic threat modeling, not compliance theater.
---

# Vendor Risk Analyzer

## Purpose

Analyze vendor security documentation (SOC 2, MSA, DPA, BAA, security questionnaires) and produce a structured risk assessment grounded in how breaches actually happen — not compliance checklist validation.

The output helps business stakeholders make informed integration decisions. Security findings are framed as risks for the business to evaluate, not as blockers.

This is the **consumer side** — evaluating someone else's SOC 2. Its producer-side mirrors are the `soc2-readiness` (author a lean program) and `soc2-audit-prep` (finalize and hand off) skills.

## Analytical Framework

This skill applies an evidence-based, adversary-informed methodology:

- **Realistic threat modeling** over compliance checkbox validation
- **Structural controls** (always-on, architectural) valued over procedural controls (dependent on human decisions)
- **Attacker cost analysis** — what's cheapest for an attacker to exploit?
- **Honest about uncertainty** — distinguish what the report proves from what it merely claims
- **Business-enabling** — security is a risk input to business decisions, not a veto

For the full threat modeling reference and breach pattern index, read `references/threat-model-index.md` before generating the threat scenarios section.

## Workflow

### Step 1: Gather Context

Before analyzing any documentation, prompt the user for the following context. Present these as questions — don't assume answers.

**About their organization:**
1. What does your organization do? (Industry, size, rough complexity)
2. What data will flow to/from this vendor? (PHI, PII, financial data, credentials, proprietary IP, etc.)
3. How critical is this integration to your operations? (Nice-to-have, important, core dependency)
4. What's your own security maturity? (Early stage / growing / mature program — this calibrates expectations)
5. Is this a new vendor evaluation or a renewal/reassessment?

**About the integration:**
6. What's the integration architecture? (API, file transfer, shared database, SSO federation, embedded iframe, full platform dependency, etc.)
7. Will the vendor store your data, process it transiently, or both?
8. Will vendor personnel have access to your systems or data directly?
9. Any regulatory requirements specific to this integration? (HIPAA, PCI-DSS, GDPR, state privacy laws, FINRA, etc.)

**About the vendor (if not evident from docs):**
10. What do you already know about this vendor? (Size, age, reputation, how they came to your attention)

If the user provides partial context or says "just analyze it," proceed with what you have and note assumptions explicitly in the report. Flag missing context that would materially change the analysis.

### Step 2: Read and Catalog the Documentation

Identify what was provided:
- SOC 2 Type I vs Type II (Type II is substantially more useful — note if only Type I was provided)
- Report period and currency (flag if >12 months old)
- Trust Services Criteria covered (Security, Availability, Confidentiality, Processing Integrity, Privacy)
- MSA/DPA/BAA terms
- Any complementary user entity controls (CUECs) or subservice organization carve-outs

Catalog what's **missing** that you'd want to see. Common gaps:
- Pen test summary or remediation evidence
- Incident history or breach notification track record
- Specific API/integration security documentation
- Subservice organization SOC 2 reports
- Data flow diagrams
- Data retention and destruction specifics

### Step 3: Analyze Through the Threat Model Lens

Read `references/threat-model-index.md` for the breach pattern framework.

For each applicable breach pattern, assess:
1. Does the vendor's described architecture create exposure to this pattern?
2. Do the controls described in the report actually mitigate it, or just adjacent risks?
3. What's the gap between what the report *claims* and what it *demonstrates*?
4. What would an attacker's cost be to exploit this path given what's described?

**Key analytical principles:**
- SOC 2 exceptions and qualified opinions are more informative than clean passes. Analyze exceptions in depth — they reveal where the organization's controls actually break down.
- Vague control descriptions ("when technically possible," "as appropriate," "in accordance with policy") are yellow flags. Note what they're hedging on.
- Subservice organization carve-outs mean those controls are NOT audited in this report. Map which parts of the stack serving the user's integration are carved out. ("Subservice organization" is a SOC-reporting concept — outsourced services whose controls are excluded from this report; "subprocessor" is a privacy/data-processing concept — a party that processes personal data on the vendor's behalf. A provider can be both, so check each under both lenses.)
- Compliance language vs. operational language: a report that reads like marketing copy is less trustworthy than one with specific technical detail.
- Distinguish between controls that protect the vendor's infrastructure and controls that protect *your data within their infrastructure*. These are often different.
- Annual pen testing and monthly vuln scanning are baseline, not differentiators. What matters is remediation velocity and scope.

### Step 4: Evaluate Contractual Terms (if MSA/DPA/BAA provided)

When MSA, DPA, or BAA documents are provided, assess:

- **Breach notification timeline** — Is it a specific, measurable window (e.g., within 72 hours)? A defined hours-based SLA is preferable for certainty; "promptly" / "without undue delay" is weaker because it's unmeasurable — though note "without undue delay" is the GDPR processor→controller standard, so it's defensible, not non-compliant, just leaves the timing to the vendor's discretion.
- **Liability caps** — Are they proportional to the data exposure risk? Caps at contract value are standard but may be inadequate for regulated data.
- **Data handling** — Retention periods, deletion/return obligations on termination, subprocessor controls, cross-border transfer mechanisms.
- **Audit rights** — Can you audit or request pen test results? Or are you limited to "review the SOC 2"?
- **Indemnification** — Who bears the cost of a breach involving your data?
- **Subprocessor notification** — Are you notified of new subprocessors? Can you object?
- **Insurance** — Cyber insurance requirements and minimums.

Flag terms that shift risk to the customer without corresponding controls.

### Step 5: Generate the Report

Produce a structured markdown report following this template:

---

```
# Vendor Risk Assessment: [Vendor Name]

**Assessment Date:** [date]
**Documents Reviewed:** [list]
**Report Period:** [period]
**Prepared For:** [requesting org, if known]
**Integration Context:** [brief description]

---

## Executive Summary

[3-5 sentences. State the bottom line: what's the overall risk posture, what are 
the 1-2 things that matter most, and is this vendor reasonable for the intended 
integration given the requesting org's context. Frame as risk for business 
decision-makers, not a pass/fail.]

## Risk Rating

**Overall: [Low / Moderate / Elevated / High]**

[1-2 sentences explaining the rating. Calibrate against industry baseline — a 
"Moderate" for a healthcare SaaS vendor means something different than for a 
marketing analytics tool.]

---

## What Matters for This Integration

[Based on the user's context: what specific data is at risk, what the 
consequences of compromise would be, and what controls in the report are 
directly relevant to protecting that data. Keep this short and specific.]

## Realistic Threat Scenarios

[For each applicable scenario from the threat model index, describe:]

### [Scenario Name]
- **Attack path:** [How this would actually happen against this vendor's described architecture]
- **Relevant controls:** [What the report says exists to prevent/detect this]
- **Gaps:** [What's missing, vague, or insufficient]
- **Your exposure:** [What happens to your org/data if this scenario plays out]
- **Estimated likelihood:** [Low/Medium/High — calibrated to vendor size, industry, 
  architecture, with reasoning]

## Exceptions and Qualified Findings

[Analyze every exception or qualification in the report. For each:]
- What failed and why it matters (or doesn't)
- Whether it affects the integration under evaluation
- Whether it suggests a systemic issue or a one-off

## Architecture and Control Assessment

### What Looks Sound
[Controls that are specific, structural, and relevant to the integration]

### What's Vague or Concerning
[Controls described in hedged language, dependent on human compliance, or 
missing for the integration path]

### What's Missing
[Controls you'd expect to see that aren't mentioned — with context on 
whether the absence is a red flag or just a reporting gap]

## Contractual Risk Assessment
[Only if MSA/DPA/BAA provided. Key gaps, unfavorable terms, missing protections.]

## Subservice Organization Risk
[Which providers are carved out, what they host, and whether their own 
attestations cover the relevant controls]

---

## Recommendations

### Before Signing / Integrating
[Specific questions to ask the vendor, documents to request, contractual 
terms to negotiate]

### Architecture Recommendations
[How to design the integration on YOUR side to limit blast radius — 
regardless of vendor controls]

### Ongoing Monitoring
[What to track, what to request annually, what would trigger reassessment]

---

## Assumptions and Limitations

[What context was missing, what assumptions were made, what this analysis 
cannot determine from the available documentation]
```

---

## Calibration Guidelines

### Industry Baselines

Calibrate findings against realistic industry expectations:

- **Healthcare IT / SaaS:** HIPAA-regulated. Expect encryption at rest and in transit, BAA, access controls, audit logging, annual risk assessments. Common weaknesses: legacy infrastructure, growth-by-acquisition inconsistency, MFA gaps in application layer, over-reliance on compliance frameworks as security strategy.
- **Financial Services / Fintech:** Heavily regulated. Higher baseline expectations for segregation of duties, change management rigor, encryption, logging. Common weaknesses: complex legacy integrations, third-party API sprawl.
- **General SaaS / Technology:** Variable. Cloud-native vendors often have better infrastructure security but may lack operational maturity (incident response, access reviews). Common weaknesses: fast-moving engineering teams that outpace security governance.
- **Government / Defense:** Expect FedRAMP, NIST 800-53 alignment. Different threat model (nation-state relevant). Common weaknesses: compliance-driven security that optimizes for auditors over attackers.
- **Retail / E-commerce:** PCI-DSS if payment data involved. Common weaknesses: large attack surface, seasonal workforce, franchise/distributed models.

Don't penalize a vendor for meeting their industry baseline. Do flag when they fall below it or when the baseline itself is insufficient for the requesting org's risk tolerance.

### Vendor Maturity Signals

Read between the lines for maturity indicators:
- **Positive:** Specific technical detail in control descriptions, NIST CSF or ISO 27001 basis (vs. ad hoc), dedicated security leadership (CISO), evidence of security architecture (not just compliance), DR testing frequency and realism.
- **Negative:** Marketing language in technical sections, "when technically possible" hedging, missing or vague incident response timelines, compliance-first framing, annual-only testing cadences for critical controls, no mention of detection/response capabilities.

### Right-Sizing the Analysis

The depth of analysis should match the integration's criticality:
- **Low criticality** (no sensitive data, easily replaceable vendor): Focus on deal-breakers only. Short report.
- **Medium criticality** (some sensitive data, moderate switching cost): Standard analysis. Cover major threat scenarios and contractual terms.
- **High criticality** (regulated data, core dependency, high switching cost): Deep analysis. All threat scenarios, contractual deep-dive, architecture recommendations, ongoing monitoring plan.

## Tone and Framing

- Write for an audience that includes both security practitioners and business decision-makers.
- Be direct. If something is bad, say it's bad. If something is fine, say it's fine and move on.
- Don't pad findings. If the vendor looks reasonable for the use case, say so.
- Frame security gaps as business risks with consequences, not as technical deficiencies.
- Never recommend rejecting a vendor solely based on SOC 2 findings — the business may have compelling reasons to accept the risk. Surface the risk clearly so they can decide.
- Avoid: "crown jewels," "cyber hygiene," "defense in depth" (as a hand-wave), "best practices" (without specifying which practice), compliance jargon without explanation.
- Prefer: specific, concrete language about what could go wrong and what it would cost.
