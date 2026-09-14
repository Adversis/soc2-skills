# Incident Disclosure Policy

**Version:** 1.0  
**Owner:** {{SECURITY_OWNER}}  
**Applies to:** Security incidents involving customer data or ACME services

---

## 1. Purpose

This policy defines when and how ACME notifies customers and regulators following a security incident.

## 2. When Notification Is Required

Customer notification is required when a security incident results in:

- Unauthorized access to, or disclosure of, customer data
- Confirmed or reasonably suspected exfiltration of personal information
- A service disruption that affects customer data integrity

Notification is not required for incidents that are fully contained without customer data exposure.

## 3. Timing

ACME aims to notify affected customers [within **72 hours** of confirming that customer data was involved]{.cmt note="72h is a chosen contractual default, not a GDPR requirement — GDPR Art. 33's 72h is authority notification, not customer notice. Contracts or regulations may require tighter timelines (24h, or 'without undue delay'); review MSAs before signing."}. Where regulatory requirements or contracts have a shorter timeline, that shorter timeline governs.

Initial notifications may be sent before full investigation is complete, and may be supplemented with additional information as the investigation progresses.

## 4. Notification Content

Customer notifications include, to the extent known at the time of notification:

- What happened and when
- What data was affected and which customers
- What ACME has done or is doing to contain and remediate
- What customers can or should do to protect themselves
- A contact for follow-up questions

## 5. Regulatory Notification

The {{SECURITY_OWNER}} determines whether regulatory notification is required based on applicable law and the nature of the data involved. Legal counsel is engaged for incidents involving personal data of EU residents (GDPR) or residents of states with breach notification laws.

## 6. Notification Authority

Only the {{SECURITY_OWNER}} (or their designated representative) is authorized to send customer or regulatory notifications. Employees must not communicate incident details to external parties without explicit authorization.

## 7. Record-Keeping

All notifications sent are preserved in the Incident Log, including the date sent, recipient list, and notification content.
