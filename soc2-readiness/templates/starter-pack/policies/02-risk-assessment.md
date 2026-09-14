# Risk Assessment Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME systems and operations

---

## 1. Purpose

This policy establishes ACME's approach to identifying, evaluating, and managing security risks to the organization and its customers.

## 2. Annual Risk Assessment

The {{SECURITY_OWNER}} conducts [a formal risk assessment at least once per year]{.cmt note="the first annual assessment must be completed and dated within the audit observation window. Risk Register will live in {{GRC_PLATFORM}}."}. The assessment:

- Identifies threats and vulnerabilities relevant to ACME's systems and data
- Evaluates the likelihood and potential impact of each risk
- Documents existing controls and their effectiveness
- Assigns ownership and a target remediation date to each accepted or unmitigated risk

Results are recorded in the Risk Register (maintained by the {{SECURITY_OWNER}}).

## 3. Risk Register

The Risk Register is a living document that captures:

- **Risk description:** What could go wrong
- **Likelihood:** Low / Medium / High
- **Impact:** Low / Medium / High
- **Current controls:** What is already in place
- **Owner:** Who is responsible for mitigation
- **Status:** Open / In Progress / Accepted / Closed

Treatment for each risk is assigned per §5.

## 4. Scope

The annual risk assessment covers at minimum:

- Customer data exposure (AI conversations, PII)
- Unauthorized access to cloud infrastructure
- Third-party vendor compromise
- Availability of core application services and data stores
- Insider threat and fraud risk (unauthorized transactions, billing abuse, insider financial misconduct)
- Phishing and social engineering

## 5. Risk Treatment

Each identified risk is assigned one of the following treatments:

- **Mitigate** — implement or improve a control to reduce likelihood or impact; assigned an owner and target date
- **Accept** — residual risk is deliberately tolerated; must be documented with written rationale and reviewed annually
- **Transfer** — risk is shifted to a third party, such as through cyber insurance or contractual liability terms
- **Avoid** — the activity or system creating the risk is discontinued

Risks rated High likelihood and High impact must have an active mitigation or transfer plan with a target completion date within 90 days of identification, unless a longer timeline is approved by the {{SECURITY_OWNER}} and documented in the Risk Register.

## 6. Triggered Reviews

A risk register review is also triggered by:

- A significant security incident
- A material change to the technology stack or data flows
- A new customer contract with elevated security requirements
- A critical or high-severity finding from an external audit or penetration test that represents a risk category not already captured in the register

Routine pentest and audit findings are tracked through the Patch Management Policy and do not require a full risk register review unless they surface a new risk category.

## 7. Relationship to Other Controls

Risk assessment findings inform:

- Vendor Management (third-party risk)
- Patch Management (vulnerability prioritization)
- Incident Response (threat modeling for detection)
- Business Continuity (availability risks)
