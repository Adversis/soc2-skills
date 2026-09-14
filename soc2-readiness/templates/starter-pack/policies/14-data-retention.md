# Data Retention Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** All ACME data systems and customer data

---

## 1. Purpose

This policy establishes how long ACME retains different categories of data and how data is securely deleted when retention periods expire.

## 2. Retention Schedule

| Data Category | Classification | Retention Period | Notes |
|--------------|---------------|-----------------|-------|
| Customer AI conversation data | Restricted | Active accounts: {{CUSTOMER_DATA_RETENTION}} (set explicitly — do not leave "indefinite"). Post-termination or on request: deleted within 30 days | Must match {{PRIVACY_URL}} and enterprise contracts; customer may request deletion at any time |
| Customer account data | Restricted | Duration of account + 30 days post-termination | Legal hold may extend |
| Authentication logs | Confidential | 1 year | Per identity provider default retention |
| Application/access logs | Confidential | 1 year | Configurable in cloud logging |
| Security incident records | Restricted | 3 years | Incident Log and post-mortems |
| Employee HR records | Restricted | Duration of employment + 7 years | Applicable employment law |
| Financial records | Confidential | 7 years | Standard accounting requirement |
| Vendor contracts | Confidential | Duration + 3 years | |

**Note:** Customer-facing retention commitments are published in ACME's privacy policy at `{{PRIVACY_URL}}`. The retention periods in the table above must remain consistent with that policy and with any enterprise customer contracts; changes to one require corresponding updates to the other.

## 3. Customer Deletion Requests

Customers may request deletion of their data at any time. ACME will fulfill deletion requests within **30 days** of a confirmed written request. Deletion is confirmed to the customer in writing.

Deletion requests for data subject to a legal hold are escalated to the {{SECURITY_OWNER}} who consults legal counsel before proceeding.

## 4. Automated Deletion

Where feasible, data deletion is automated based on the retention schedule above. The {{SECURITY_OWNER}} confirms that automated deletion is functioning as part of the annual risk assessment. Where automation is not in place, manual deletion is documented.

## 5. Secure Deletion

When data is deleted, it must be removed from:

- Primary databases
- Backups — note that backup deletion may lag the primary deletion by the backup retention period. This is documented and acceptable.
- Any copies in cloud storage or other secondary storage
- Logs — log data may be separately retained per the schedule above even if the underlying record is deleted

Cloud provider-managed services use provider-standard secure deletion for storage media. ACME does not manage physical media directly.

## 6. End of Customer Relationship

Upon termination of a customer account, ACME:

- Deletes or exports customer data per the customer's instructions, within 30 days
- If no instructions are received, deletes customer data after 30 days of account termination
- Confirms deletion to the customer in writing

## 7. Device and Media

Employee device data is addressed in the Physical Security Policy. No customer data should reside on employee devices; if it does, it must be deleted immediately per this policy.
