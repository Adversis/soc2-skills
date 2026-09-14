# Vendor Management Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All third-party vendors and service providers with access to ACME systems or data

---

## 1. Purpose

This policy governs how ACME evaluates, approves, and monitors third-party vendors to manage the risk of data exposure and service disruption.

## 2. Vendor Register

The {{SECURITY_OWNER}} maintains a [Vendor Register]{.cmt note="maintained in {{GRC_PLATFORM}} (vendor & risk management pages). Auditor will request an export; confirm every material vendor (cloud provider, managed data stores, model/inference provider, identity provider, source control) is present with a SOC 2 report (or alternate review evidence)"} listing all vendors that process, store, or transmit ACME data or have access to ACME systems. At minimum, the register captures:

- Vendor name and service description
- Data shared or systems accessed
- Security assessment status and date
- Annual review date
- DPA or contract reference, if the vendor processes customer data

## 3. Pre-Approval Security Assessment

Vendors are assigned a risk tier at onboarding using two questions: (1) if this vendor were breached, would customer data be exposed? (2) if this vendor went down, would the ACME product stop working?

"Customer data" in question 1 means customer AI conversation data and customer account information (e.g., names, emails, billing details). Vendors processing either type are Critical.

| Tier | Criteria | Review depth |
|---|---|---|
| Critical | Yes to either question | SOC 2 report and note applicable CUECs |
| Standard | Holds source code or internal Restricted data (e.g., employee records, credentials); No to both | SOC 2 report or questionnaire |
| Light | Neither | Confirm service description and data shared |

Tier is recorded in the Vendor Register and sets the depth of onboarding and annual review.

The {{SECURITY_OWNER}} conducts a security review of vendors at Critical or Standard tier. Acceptable forms of evidence include:

- Current SOC 2 Type I or Type II report (preferred)
- ISO 27001 certification
- Completed security questionnaire (e.g., CAIQ or VSAQ)

Vendors unable to provide any of the above may be approved with documented rationale and compensating controls. Major cloud infrastructure providers are assessed based on their published compliance documentation and SOC 2 reports.

Critical tier vendors processing customer data must have a Data Processing Agreement or confidentiality terms before access is granted.

## 4. Annual Review

All vendors in the Vendor Register are reviewed annually. Review depth follows the vendor's tier (see procedures/vendor-review-questions.md). At minimum, every review confirms:

- The vendor relationship is still active and necessary
- The data shared and access granted is still appropriate

For Critical and Standard tier vendors, the review also confirms the vendor's security posture remains acceptable. Acceptable evidence includes an updated SOC 2 report, equivalent third-party attestation, or completed security questionnaire. Where a vendor cannot provide formal assurance, the {{SECURITY_OWNER}} documents a brief risk assessment (data shared, exposure, compensating controls) as the review record.

The outcome of each annual review is recorded in the Vendor Register.

## 5. Vendor Offboarding

When a vendor relationship ends, the {{SECURITY_OWNER}} ensures:

- ACME credentials and API keys granted to the vendor are revoked
- The vendor is instructed to delete or return ACME data per the contract
- The vendor is removed from the Vendor Register (or marked inactive with an end date)

## 6. Subprocessors

ACME maintains a current list of material subprocessors (vendors that process customer data on ACME's behalf), kept separately from this policy and updated as vendor relationships change. Customers are notified of subprocessor changes per applicable contracts and privacy commitments.
