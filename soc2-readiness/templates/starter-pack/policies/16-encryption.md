# Encryption Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME systems and data

---

## 1. Purpose

This policy establishes requirements for encryption of data at rest and in transit at ACME.

## 2. Encryption in Transit

All data transmitted over public networks must be encrypted using TLS 1.2 or higher. This includes:

- All customer-facing API endpoints (enforced at the infrastructure layer)
- All connections from application services to data stores (TLS enforced by providers)
- All connections to third-party services
- Employee access to cloud infrastructure management consoles and CLIs

Unencrypted HTTP traffic to production endpoints is prohibited. Redirects to HTTPS are enforced at the infrastructure layer.

## 3. Encryption at Rest

Customer data and all Restricted data is encrypted at rest using cloud provider-managed encryption (AES-256), which is enabled by default across the cloud platform's managed storage services. This includes:

- Managed object storage
- Managed block storage (if used)
- Managed relational databases (if used)

Managed data platforms used by ACME encrypt data at rest by default per their platform specifications. The {{SECURITY_OWNER}} confirms encryption-at-rest status for each material data platform during annual vendor reviews.

Employee device storage is encrypted via full-disk encryption, enforced by MDM (see Physical Security Policy).

## 4. Key Management

ACME relies on [cloud provider-managed encryption keys or customer-managed keys if required]{.cmt note="confirm provider-managed keys are sufficient for current customers."} for most storage. Customer-managed encryption keys are not required at this stage but will be evaluated if enterprise customer contracts require it.

The {{SECURITY_OWNER}}:

- Documents which key management approach is used for each data store
- Reviews key management configuration annually
- Ensures no encryption keys are stored in application code or unencrypted configuration files

Application secrets and API keys are stored in a dedicated secrets management system, not in environment files or source code.

## 5. TLS Certificate Management

TLS certificates for public-facing domains are managed via an automated certificate management service. Certificate expiry is monitored. Manual certificate renewals, if any, are tracked in the Change/Release Log.

## 6. Prohibited Configurations

The following are prohibited:

- TLS versions below 1.2 on any public-facing endpoint
- Self-signed certificates on production endpoints accessible by customers
- Storing encryption keys or secrets in source control repositories (including private repositories)
- Disabling encryption-at-rest on managed cloud storage services without {{SECURITY_OWNER}} approval and documented compensating controls

## 7. Scope Limitations

This policy covers data within ACME's control. ACME relies on its cloud providers' and vendors' encryption practices for data within their internal infrastructure. These are evaluated through vendor security reviews per the Vendor Management Policy.
