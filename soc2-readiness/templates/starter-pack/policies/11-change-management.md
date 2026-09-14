# Change Management Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All changes to ACME production systems and code

---

## 1. Purpose

This policy governs how changes to production code and infrastructure are reviewed and deployed to reduce the risk of introducing security vulnerabilities or service disruptions.

## 2. Scope

This policy applies to:

- Application code deployed to production
- Infrastructure configuration (cloud platform and managed data services)
- Changes to CI/CD pipelines
- Changes to authentication or authorization configuration

## 3. Pull Request Review

All production code changes must be submitted via a pull request and approved by at least one reviewer before merging. The reviewer is responsible for:

- Reviewing the change for correctness and security
- Confirming that tests pass
- Approving the PR before merge

**Sole-operator changes.** Where a production code or infrastructure change is made by the only available qualified engineering reviewer (today, the {{SECURITY_OWNER}}), pre-deployment peer review is replaced by the following compensating controls:

1. A structured self-review checklist completed at the time of the change, covering scope, blast radius, rollback path, credential exposure, security-posture impact, and (for production-affecting changes) staging validation and before/after evidence. The checklist is embedded in the `infra-change` Issue template for non-code changes and in the pull request description for code/Terraform changes.
2. Monthly batch review by the COO of all sole-operator changes (merged PRs and closed `infra-change` Issues), with sign-off recorded.

This pattern applies until a second qualified engineering reviewer is available.

Self-approval outside this pattern remains prohibited except in documented emergencies (see §5).

## 4. Branch Protection

The `main` branch (or equivalent production branch) is protected. Direct pushes are prohibited. All changes go through pull requests.

Branch protection settings are maintained by the {{SECURITY_OWNER}} and reviewed if changed.

## 5. Emergency Changes

In a Severity 1 incident, a change may be deployed to production without a pre-deployment peer review. When this occurs:

- The change must be documented in the incident record at the time of deployment
- A retroactive pull request and peer review must be completed within **two business days**

The {{SECURITY_OWNER}} approves all emergency changes.

## 6. Infrastructure Changes

Changes to cloud infrastructure configuration and managed data service settings follow the same PR review process where managed as infrastructure-as-code. Manual changes made directly in cloud consoles must be:

- Documented as a {{VCS}} Issue using the Infrastructure / System Change template (`infra-change` label) at the time of the change, including the self-review checklist
- Followed by a corresponding infrastructure-as-code update where applicable

## 7. Deployment

Deployments to production are performed via the CI/CD pipeline. Manual deployments (outside CI/CD) must be approved by the {{SECURITY_OWNER}} and documented.

## 8. Rollback

Each deployment must support rollback to the prior version. The mechanism for rollback is documented in the Backup Recovery Runbook. In the event of a failed deployment causing production impact, the team reverts before investigating root cause.
