# Physical Security Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME employees, devices, and work locations

---

## 1. Purpose

This policy establishes physical security controls for ACME's work environments and devices. ACME has no company-owned data center or server room. All production infrastructure runs in managed cloud environments operated by third-party providers. Physical security controls therefore focus on employee devices and work locations.

## 2. Work Locations

Employees may work from company offices, co-working spaces, and home offices. When working in public or shared spaces, employees must:

- Not leave devices unattended and unlocked
- Not conduct sensitive conversations (customer data, security incidents) where they can be overheard
- Ensure that network traffic is encrypted in transit (see Encryption Policy)

## 3. Device Management

Company-issued devices should be enrolled in Mobile Device Management (MDM) where available. MDM is the preferred mechanism for enforcing and verifying the following controls:

- Full-disk encryption enabled
- Screen lock after 15 minutes of inactivity, with password required to unlock
- Remote wipe capability
- Automatic OS security updates
- Anti-malware and endpoint detection-and-response (EDR) protections are enabled and kept current, layering platform-native protections with a managed EDR agent

Where a device is not MDM-enrolled, the same controls must be manually configured by the employee and verified via our inventory tool. The {{SECURITY_OWNER}} is responsible for confirming compliance during onboarding and the annual device review.

Personal devices (BYOD) may not access Restricted data or production systems without {{SECURITY_OWNER}} approval and equivalent controls.

## 4. Asset Register

The COO maintains an Asset Register of all company-issued devices, recording device type and serial number, the assigned employee, and management/enrollment status. The register is updated when a device is issued, returned, sanitized, or decommissioned. For returned or decommissioned devices, the sanitization method and the person who verified it are recorded.

Physical access (building and floor badges, and keys to ACME's space) is provisioned and recorded by building management; badge and key requests and recoveries are retained in the onboarding and offboarding records.

## 5. Device Handling

Employees must:

- Not leave laptops unattended in vehicles or public spaces overnight
- Report lost or stolen devices to the {{SECURITY_OWNER}} immediately
- Return all company devices upon departure (see Offboarding Checklist)

## 6. Lost or Stolen Devices

A lost or stolen device is treated as a security incident. The {{SECURITY_OWNER}}:

- Revokes the employee's corporate identity provider account, which removes downstream access to all federated systems
- Issues a remote wipe command via MDM if the device is enrolled
- Revokes any credentials stored on the device that are not covered by account revocation (API keys, SSH keys, tokens)
- Logs the incident in the Incident Log
- Confirms with the employee what data was accessible on the device at the time of loss

## 7. Device Disposal

Before disposing of any company device:

- Full-disk wipe is performed (cryptographic erase is acceptable)
- The device is removed from MDM if enrolled
- The Asset Register is updated

Devices must not be disposed of in general trash. They are returned to the manufacturer's recycling program, donated to a reputable program, or disposed of by a certified e-waste vendor.

## 8. Physical Media

ACME does not use physical media (USB drives, external hard drives) for storing customer data. If physical media must be used for a specific purpose, it must be encrypted and stored securely when not in use.

## 9. Office Access

ACME's office is in a managed co-working facility with three layered physical access controls:

1. Building entry is gated by badge access managed by building management
2. The floor entrance requires a separate badge
3. ACME's dedicated office and conference room are secured by physical key, issued only to ACME employees

Badge access and physical keys are issued during onboarding and revoked or recovered during offboarding (see Onboarding Checklist and Offboarding Checklist). Building management maintains the badge and key records; ACME retains the request and recovery correspondence in the onboarding and offboarding records.

## 10. Visitors

ACME notifies building reception in advance by email with the names of expected visitors. Reception verifies the visitor's identity on arrival, records the visit in the building visitor log, and contacts the ACME host. The host meets the visitor at reception and escorts them within ACME's space for the duration of their visit. Visitor logs are maintained by building management and available on request.
