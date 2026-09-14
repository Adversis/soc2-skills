# Patch Management Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME systems, devices, and software dependencies

---

## 1. Purpose

This policy establishes how ACME identifies and remediates known vulnerabilities in its software, dependencies, and infrastructure.

## 2. Scope

This policy covers:

- Application dependencies (libraries, packages)
- Container base images and runtimes (if applicable)
- Employee devices
- Cloud infrastructure managed directly by ACME

Managed cloud services are patched by their respective providers. ACME confirms patching cadence as part of annual vendor reviews.

## 3. Vulnerability Identification

Vulnerabilities are identified through:

- Automated dependency scanning in CI
- Cloud provider security findings (where enabled)
- External security advisories (received from vendors)
- Findings from security assessments

## 4. Security Assessments and External Scanning

ACME performs periodic security assessments of its external-facing, in-scope infrastructure at least annually and after significant changes. A security assessment may take the form of an independent third-party penetration test, an internal or third-party vulnerability assessment, or another structured security evaluation.

Dependency vulnerability scanning runs continuously on merge requests via automated tooling in CI. [External-facing (DAST/network) scanning is stated here only if ACME actually runs it — give the real cadence and keep the dated output. If it is not yet deployed, do not commit a cadence: the periodic security assessment above plus CI dependency and static-analysis scanning are the compensating controls for a first Type I.]{.cmt note="Don't commit to a monthly external scan unless it is deployed and producing dated evidence — it contradicts the deferred/compensating-control posture in the auditor-Q&A guidance and manufactures a finding."}

Vulnerabilities identified by any source are recorded, risk-rated, and tracked in the source control platform's security dashboard and, where needed, in a supplementary vulnerability tracker maintained by the {{SECURITY_OWNER}}. Remediation timelines and exception handling are governed by Remediation Timelines.

## 5. Remediation Timelines

Remediation priority is determined by a combination of severity, exploit availability, and system exposure — not CVSS score alone.

**Risk factors:**
- **Exploit availability:** Whether a working exploit is publicly known or actively used in the wild. Primary reference: CISA Known Exploited Vulnerabilities (KEV) catalog and public proof-of-concept availability.
- **System exposure:** Whether the affected component is internet-facing (directly reachable by external parties) or internal-only (no direct external access).

**Default targets** (measured from date vulnerability is confirmed present in an ACME-used component):

| Severity | Actively exploited | Internet-facing, no known exploit | Internal-only, no known exploit |
|----------|--------------------|-----------------------------------|---------------------------------|
| Critical (CVSS ≥ 9.0) | 14 days | 30 days | 90 days |
| High (CVSS 7.0–8.9) | 14 days | 60 days | 180 days |
| Medium (CVSS 4.0–6.9) | 30 days | 90 days | 365 days |
| Low (CVSS < 4.0) | 90 days | Best effort | Best effort |

"Actively exploited" means listed in the CISA KEV catalog, or a working public PoC exists and exploitation is plausible given ACME's environment.

**Exceptions:** When a target cannot be met — no patch available, upgrade requires breaking changes, or a risk assessment determines exploitation is implausible — the {{SECURITY_OWNER}} documents: the vulnerability, its exposure and exploit status, compensating controls in place, and a revised target.

## 6. Device Patching

Employee macOS devices must:

- Have automatic security updates enabled for the operating system
- Have all applications kept current; critical patches applied within 14 days of release

The {{SECURITY_OWNER}} reviews device patch status at least quarterly.

## 7. Auto-Updates

Where auto-update is available and operationally safe (e.g., OS security patches, non-production tooling), it should be enabled. Production application dependencies are updated through the standard CI/CD pipeline with testing, not auto-updated in place.

## 8. End-of-Life Software

Software that has reached end-of-life (no security patches available) must not be used in production. The {{SECURITY_OWNER}} tracks EOL dates for ACME's key components and plans upgrades ahead of EOL dates.
