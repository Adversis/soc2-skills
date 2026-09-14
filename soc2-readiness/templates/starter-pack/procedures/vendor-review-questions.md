# Vendor Review Questions

Reference for onboarding and annual vendor reviews. Record answers in the {{GRC_PLATFORM}} vendor entry. Depth scales with tier (see Vendor Management Policy §3).

---

## Onboarding

1. What ACME or customer data does this vendor access, store, or process?
2. Assign tier: if breached, is customer data exposed? If down, does the product stop? (Critical = yes to either; Standard = Restricted data only; Light = neither)
3. Attestation on file? In preference order: SOC 2 Type II → ISO 27001 → security questionnaire (CAIQ/VSAQ) → documented risk rationale. Note type, date, and coverage period. If none available, document compensating controls and {{SECURITY_OWNER}} approval.
4. Opinion clean? Any exceptions relevant to ACME? (Skip if no SOC 2.)
5. **Critical tier + SOC 2 only:** Which CUECs from their report apply to ACME? Record a short bullet list in the {{GRC_PLATFORM}} vendor Notes field — one line per CUEC noting the applicable ACME control. No separate document needed.
6. DPA or confidentiality terms in place if they process customer data?

## Annual Review

1. Is the relationship still active and the access still necessary and appropriate?
2. Updated attestation on file? Note if the report year changed. If the vendor still has no formal attestation, re-evaluate risk rationale.
3. Any new exceptions or material changes from last year's report? (Skip if no SOC 2.)
4. **Critical tier + SOC 2 only:** Any new CUECs since last review?
