# SOC 2 Type I starter pack (ACME'd)

A complete, lean, **battle-tested** document set for a small cloud-native company's first SOC 2 Type I
(Security TSC). It is a real Type I engagement's deliverables (anonymized), genericized to the sample company
**ACME** with `{{TOKENS}}` for client-specific values. Use it to start a new engagement from a
fill-in-the-blanks base instead of from a GRC platform's ~21 generated templates.

Provenance: adapted from [Tailscale's open security policies](https://github.com/tailscale/security-policies)
(MIT), then refined through a live Type I to the lean/honest bar this skill teaches. ~17 policies (~40 pp,
~8,000 words) vs. the ~21-policy (~107 pp) platform default it replaced.

## How to use

1. Copy `starter-pack/` into the new engagement's working directory.
2. Work through `REPLACE.md`: find/replace `ACME`, fill the `{{TOKENS}}`, confirm the calibrated defaults.
3. **Confirm the real stack first** (the CTO confirmation pass — `../../SKILL.md` phase 1). Fill tokens
   from confirmed facts, not assumptions.
4. Delete what doesn't apply. Every control you keep is a control the auditor tests.

## What's inside

```
starter-pack/
  README.md              # this file
  REPLACE.md             # the fill manifest: tokens, defaults, confirmation checklist
  system-description.md  # SOC 2 Type I system description template
  policies/              # 17 policies + 00-meta (scope/ownership/TSC map) + responsibilities
  procedures/            # checklists, access-control matrices, runbooks, IR templates, security page
  registers/             # live logs + risk register (header-only) + risk-register-SAMPLES + README
  optional/              # cuec-tracker.csv — optional depth, kept out of the default deliverable
```

## The doctrine baked into this pack (don't undo it)

- **Policies name controls; procedures name products.** The only product named in a policy is `{{VCS}}`,
  deliberately — because PRs are the change records. Everything else is functional/generic so a tool
  swap never triggers a policy revision. Keep it that way.
- **Lean over comprehensive.** A process you describe but don't run is worse than nothing — it's a
  self-inflicted finding. Match every doc to what the company actually does.
- **Calibrated defaults** are chosen to survive a Type 2, not to sound impressive. Confirm, don't inflate.
- **Never backdate.** For periodic controls not yet run, "designed and implemented; first cycle scheduled
  for [date]" is honest and sufficient. See `../../references/type1-evidence.md`.

## Related skill references

- `../../references/policy-altitude.md` — why the set is shaped this way; calibrated-commitment rationale.
- `../../references/type1-evidence.md` — as-of discipline, "not yet due" framing, the five-part shape.
- `../../references/auditor-qa-patterns.md` — the evidence-request loop and reusable responses.
- `../../references/execution-playbook.md` — running the engagement (critical path, platform, scheduling).
- `../evidence-index.md` — the control → artifact → date → status grid to maintain alongside these docs.
