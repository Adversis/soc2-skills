# Access Control Matrix

**Owner:** {{SECURITY_OWNER}}  
**Last reviewed:** _YYYY-MM-DD_  
**Reviewed by:** _Name_

ACME uses **role-based access control (RBAC)**. The access each person has is determined by their role; exceptions are recorded explicitly per person.

The Access Control Matrix is a single workbook with four tabs:

| Tab | Contents |
|------|----------|
| Roles | Definition of each role: what access the role gets in each system |
| People | One row per person: name, role, status, joined / departed dates, exceptions |
| Auth | One row per system: auth method, MFA source, network posture, notes |
| Service Accounts | Index of non-human identity categories with pointer to each system's native inventory |

## How access is derived

To determine what a given person has access to:

1. Look up the person in the **People** tab to find their Role
2. Look up the Role in the **Roles** tab to find access per system
3. Apply any **Exceptions** noted on the person's row (temporary admin, on-call elevation, etc.)

The role definition is the policy. The per-person Exceptions column is the only place where access can deviate.

## Access levels used

- **{{CLOUD_PROVIDER}}:** Owner, Editor, Viewer, Custom
- **{{VCS}}:** Org Admin, Member, Outside collaborator
- **{{DATA_STORE}}:** Admin, Read/Write, Read-only _(one line per managed data platform)_
- **{{IDP}} Admin:** Admin, User

## Authentication

MFA is enforced via {{IDP}} 2SV for all federated systems; verified at onboarding. Per-system authentication details (federated vs standalone, MFA source, network posture) are in the **Auth** tab.

## Role changes and exceptions

Changes to a person's Role in the **People** tab, or additions to the **Exceptions** column, require written approval from the {{SECURITY_OWNER}} (or COO when the change affects the {{SECURITY_OWNER}}). The approval may be an email, a {{CHAT_PLATFORM}} message captured to the documentation system, or a note on the Onboarding/Offboarding checklist. The People tab is updated only after that written approval exists, and the approval is retained as evidence of the access change. The change is then recorded in the access-change log (`access-change-log.csv`): date, prior and new role, systems affected, and approver.

Temporary elevations (on-call admin, debugging access) are recorded in the Exceptions column with a target end date and removed at that date.

## Reconciliation

At each annual access review the {{SECURITY_OWNER}} exports actual access from each platform and reconciles against the expected access derived from Roles + People + Exceptions. Discrepancies are resolved at the platform.
