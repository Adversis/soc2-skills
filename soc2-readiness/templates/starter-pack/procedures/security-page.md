# Security at ACME

Last updated: 2026-05-29

ACME maintains a security program designed to protect customer data and the integrity of the ACME platform. This page summarizes our public commitments. Additional detail and current attestations are available on our Trust Page at trust.acme.ai (or request via {{SECURITY_CONTACT}}).

## Security Commitments

### Data Protection

- Customer data is encrypted in transit using TLS 1.2 or higher
- Customer data is encrypted at rest using AES-256
- Customer environments are logically isolated
- Production infrastructure is hosted in a major cloud platform whose own security controls are evaluated annually

### Access Control

- Production system access is restricted on a least-privilege basis
- Multi-factor authentication is required for production system access
- Personnel access is reviewed at least annually and revoked on departure or role change

### Personnel

- Pre-employment security screening (identity confirmation and reference checks) is completed before access to ACME systems is granted; work-authorization is verified as part of the employment process in accordance with applicable law
- Personnel acknowledge confidentiality and security policies at hire and at least annually
- Annual security awareness training is required

### Software Development

- Production code changes are reviewed and approved before merge
- Automated dependency scanning and secret scanning are required on production code changes
- Production branches are protected

### Incident Response

- ACME maintains a documented incident response program and severity classification
- Customers are notified of confirmed security incidents involving their data in accordance with applicable law, applicable contracts, and the Incident Disclosure Policy
- The incident response process is exercised at least annually

### Vendor Management

- Critical vendors processing customer data are subject to security review at onboarding and annually thereafter
- Data processing agreements or equivalent confidentiality terms are in place with vendors processing customer data
- A current list of subprocessors is maintained and made available to customers

### Business Continuity

- Production data is backed up by managed cloud services
- Backup restoration is tested annually
- Recovery objectives are documented and reviewed at least annually

### Audit and Compliance

ACME maintains a compliance program that includes independent third-party examinations and assessments of its security controls. Current attestations, certifications, and examination reports are listed on the Trust Page at trust.acme.ai and are available to current and prospective customers under NDA on request.

## Reporting a Security Issue

Email {{SECURITY_CONTACT}} with a description of the issue, where it was observed, how it can be reproduced, and a contact for follow-up.

ACME does not operate a bug bounty program and does not invite or solicit security testing of its systems, infrastructure, or user accounts. Any testing beyond the good-faith terms below is unauthorized and may violate our Terms of Service and applicable law.

Good-faith reporting. ACME will not pursue or support legal action against anyone whose research and reporting comply with all of the following:

- identifies a vulnerability through interaction with ACME's own publicly accessible interfaces,
- does not access, modify, exfiltrate, retain, or disclose customer data beyond the minimum necessary to demonstrate the issue,
- does not degrade, disrupt, or impair the availability of ACME services or the experience of other users,
- does not use social engineering, physical attacks, or attacks against ACME personnel, and
- reports the issue to {{SECURITY_CONTACT}} promptly and does not publicly disclose it until ACME has had a reasonable opportunity to remediate.

Conduct that complies with these terms is authorized within the meaning of the Computer Fraud and Abuse Act, the Digital Millennium Copyright Act, and analogous state laws, and ACME waives any conflicting restriction in its Terms of Service for that limited purpose. This is a commitment by ACME only; it does not bind, and cannot waive the rights of, any third party or government authority. If a claim is brought against a reporter who complied with these terms, ACME will, on request, confirm in that proceeding that the activity was conducted consistent with this policy. ACME reserves all rights with respect to activity outside these terms.

If the issue concerns an ACME subprocessor, please report to the subprocessor directly and copy ACME only if the issue may affect data shared with ACME.

ACME does not commit to a specific external acknowledgment or response timeline. We may follow up if additional information is needed or if there is a material update to share.

Reports are submitted under ACME's Terms of Service. You grant ACME a non-exclusive, royalty-free, perpetual, irrevocable license to use, reproduce, and act on the report for security purposes. Submission creates no partnership, agency, or employment relationship and entitles you to no compensation. ACME reserves all rights at law and in equity.

## Contact

{{SECURITY_CONTACT}}
