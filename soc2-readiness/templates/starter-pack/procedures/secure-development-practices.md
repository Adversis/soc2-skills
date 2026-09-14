# Secure Development Practices

**Internal reference** — not a policy. Describes how ACME engineers build and ship software.  
Governed by: Change Management Policy, Testing Policy

---

## How We Work

All code and infrastructure lives in {{VCS}}. Development happens on feature branches; nothing goes to `main` directly. Every change ships via pull request.

## Pull Request Requirements

- At least one approval from someone other than the author before merge
- All CI checks must pass: unit/integration tests, dependency vulnerability scan, build validation
- `main` is branch-protected; force pushes are disabled

## CI Pipeline

Every PR runs:

- Automated tests (unit + integration)
- Dependency vulnerability scan (flags known CVEs in third-party packages)
- Build and container image validation

Failures block merge. Results are visible in the PR.

{{VCS}} Actions workflows run with minimal permissions and no secrets by default. Secrets required for deployment are scoped to specific workflows and environments, not available repo-wide.

## Infrastructure Changes

Infrastructure is managed as Terraform code, reviewed through the same PR process as application code. Manual changes made directly in cloud consoles are promptly documented in the Change/Release Log and followed up with a code change.

## Security Review Triggers

Before shipping to production, a security review is conducted for changes that:

- Introduce new external APIs or webhooks
- Add or modify authentication or authorization logic
- Create new flows involving customer data
- Integrate a new third-party service

The review is lightweight — a PR comment or linked {{VCS}} issue documenting what was considered. It does not require a separate meeting or formal sign-off.

## Credentials & Secrets

No secrets, credentials, API keys, or tokens are committed to source code or stored in environment files in repositories. Secrets are stored in {{SECRETS_MANAGER}} or equivalent secrets management tooling and injected at runtime.

For developer authentication to critical systems ({{CLOUD_PROVIDER}} console, {{VCS}}, production infrastructure), hardware security keys are preferred over software-based MFA. Long-lived personal access tokens are avoided; short-lived tokens and hardware-bound SSH keys are preferred where the system supports them.

## Production Access & Monitoring

Direct access to production data stores and customer data is limited to engineers who need it for a specific operational purpose and requires {{SECURITY_OWNER}} awareness. Access to production data stores ({{DATA_STORE}}, {{DATA_STORE}}) and IAM administration ({{CLOUD_PROVIDER}} IAM, {{SECRETS_MANAGER}}) is logged via the platform's audit logs and available for review.

No customer data is used in development or staging environments.

## Environments

Development and testing happen in environments isolated from production.

## Dependency Management

Third-party dependencies are scanned on every PR. Findings are triaged and remediated per Patch Management Policy timelines.

## Emergency Changes

If a Sev 1 incident requires a change outside the normal PR process, the change is documented in the incident record at deploy time and a retroactive PR is opened promptly for peer review.
