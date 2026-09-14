# Information Classification Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME employees and contractors

---

## 1. Purpose

This policy defines how ACME classifies information so that data is handled with appropriate care based on its sensitivity.

## 2. Classification Levels

### Public

Information that is publicly available or intended for public distribution.

Examples: marketing materials, public documentation, job postings, open-source code.

Handling: No restrictions.

### Confidential

Internal business information whose unauthorized disclosure could cause operational disruption, reputational harm, or competitive disadvantage, but which does not directly expose individuals.

Examples: internal roadmaps, business plans, employee contact lists, financial projections, vendor contracts, aggregate usage metrics.

Handling: Do not share externally without explicit approval. Use company-managed accounts and storage.

### Restricted

Information whose unauthorized disclosure could cause significant harm to individuals, violate regulatory obligations, or breach customer trust.

Examples:
- Customer AI conversation data (may contain PII, PHI, or confidential business information)
- Authentication credentials and API keys
- Cryptographic keys
- Security incident details
- Personal employee information (compensation, performance reviews)

Handling:
- Access limited to personnel with a business need
- Must not be transmitted over unencrypted channels
- Must not be stored on personal devices or personal cloud accounts
- Must be deleted per the Data Retention Policy when no longer needed
- Breaches involving Restricted data must be treated as security incidents

## 3. Default Classification

When in doubt, treat information as Confidential. Customer data that may contain user-provided content is always treated as Restricted.

## 4. Labeling

Formal document labeling is not required at this stage. Classification is applied based on content type as defined above. Employees are responsible for knowing the classification of information they work with.

## 5. Handling Customer Data

ACME's core product processes customer-provided text (AI conversations). This data is always Restricted. Employees must not:

- Access customer conversation data except for a documented business reason (e.g., debugging a reported issue)
- Copy customer data to personal accounts, devices, or unmanaged storage
- Use customer data to train or fine-tune models without explicit customer consent
