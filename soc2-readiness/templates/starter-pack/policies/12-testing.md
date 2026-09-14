# Security Testing Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME production code and infrastructure

---

## 1. Purpose

This policy establishes ACME's requirements for security testing as part of software development and infrastructure management.

## 2. CI/CD Requirements

All code merged to the production branch must pass automated CI checks before deployment. CI pipelines are version-controlled alongside application code and at minimum include:

- Automated test suite (unit and/or integration tests)
- Automated dependency vulnerability scanning
- Build validation

Failed CI checks block deployment. Bypassing CI is prohibited without {{SECURITY_OWNER}} approval and must be documented.

## 3. Infrastructure as Code

Production infrastructure configuration is managed as code and stored in version control. Changes to infrastructure-as-code follow the same PR review process as application code.

## 4. Dependency Management

Third-party dependencies are reviewed for known vulnerabilities as part of CI. The {{SECURITY_OWNER}} reviews open vulnerability alerts at least monthly and prioritizes remediation per the Patch Management Policy.

## 5. Pre-Production Testing

Significant features and changes are tested in a non-production environment before promotion to production, where one is available. The {{SECURITY_OWNER}} defines what constitutes "significant" based on the change's scope and risk.

## 6. Penetration Testing

ACME will commission an external penetration test as required by enterprise customer contracts or as the {{SECURITY_OWNER}} determines appropriate. Findings from penetration tests are tracked as {{VCS}} Issues and remediated per the Patch Management Policy.

## 7. Security Review for New Features

For features that involve:

- New external-facing APIs or endpoints
- Processing or storing customer data in a new way
- New authentication or authorization logic
- Third-party integrations with data access

The {{SECURITY_OWNER}} or a designated team member conducts a security review before the feature is deployed to production. The review is documented as a comment in the PR or a linked {{VCS}} Issue.
